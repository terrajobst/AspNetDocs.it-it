---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: Panoramica della modifica e dell'eliminazione dei dati in DataList (VB) | Microsoft Docs
author: rick-anderson
description: Mentre DataList non dispone di funzionalità predefinite di modifica ed eliminazione, in questa esercitazione verrà illustrato come creare un DataList che supporti la modifica e l'eliminazione di o...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 49b09cbf0f12f7c7233c3bb2a8b3b2c073bf117e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74573069"
---
# <a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Panoramica della modifica e dell'eliminazione dei dati in DataList (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) o [scaricare il file PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Mentre DataList non dispone di funzionalità predefinite di modifica ed eliminazione, in questa esercitazione verrà illustrato come creare un DataList che supporta la modifica e l'eliminazione dei dati sottostanti.

## <a name="introduction"></a>Introduzione

In [una panoramica dell'esercitazione sull'inserimento, l'aggiornamento e l'eliminazione dei dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) è stato esaminato come inserire, aggiornare ed eliminare dati utilizzando l'architettura dell'applicazione, un oggetto ObjectDataSource e i controlli GridView, DetailsView e FormView. Con ObjectDataSource e questi tre controlli Web di dati, l'implementazione di semplici interfacce di modifica dei dati era un blocco ed era necessario semplicemente selezionare una casella di controllo da uno smart tag. Non è necessario scrivere codice.

Sfortunatamente, DataList non dispone delle funzionalità di modifica e eliminazione predefinite inerenti al controllo GridView. Questa funzionalità mancante è dovuta al fatto che DataList è un Relic della versione precedente di ASP.NET, quando i controlli origine dati dichiarativi e le pagine di modifica dei dati senza codice non sono disponibili. Anche se DataList in ASP.NET 2,0 non offre le stesse funzionalità di modifica dei dati predefinite di GridView, è possibile usare le tecniche ASP.NET 1. x per includere tali funzionalità. Questo approccio richiede un po' di codice, ma, come si vedrà in questa esercitazione, il DataList presenta alcuni eventi e proprietà per facilitare il processo.

In questa esercitazione verrà illustrato come creare un DataList che supporta la modifica e l'eliminazione dei dati sottostanti. Nelle esercitazioni future vengono esaminati gli scenari di modifica e eliminazione più avanzati, inclusa la convalida dei campi di input, la gestione corretta delle eccezioni generate dai livelli di accesso ai dati o della logica di business e così via.

> [!NOTE]
> Analogamente a DataList, il controllo Repeater non dispone della funzionalità predefinita per l'inserimento, l'aggiornamento o l'eliminazione. Sebbene sia possibile aggiungere questa funzionalità, DataList include proprietà ed eventi non trovati nel Repeater che semplificano l'aggiunta di tali funzionalità. Quindi, questa esercitazione e quelle future che esaminano la modifica e l'eliminazione si concentrano esclusivamente sul DataList.

## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Passaggio 1: creazione delle pagine Web delle esercitazioni di modifica ed eliminazione

Prima di iniziare a scoprire come aggiornare ed eliminare i dati da un DataList, è necessario prima di tutto creare le pagine ASP.NET nel progetto del sito Web che saranno necessarie per questa esercitazione e le successive. Per iniziare, aggiungere una nuova cartella denominata `EditDeleteDataList`. Aggiungere quindi le pagine ASP.NET seguenti a tale cartella, assicurandosi di associare ogni pagina alla pagina master `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Aggiungere le pagine ASP.NET per le esercitazioni](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni

Analogamente alle altre cartelle, `Default.aspx` nella cartella `EditDeleteDataList` elenca le esercitazioni nella sezione. Si ricordi che il controllo utente `SectionLevelTutorialListing.ascx` fornisce questa funzionalità. Quindi, aggiungere questo controllo utente a `Default.aspx` trascinandolo dall'Esplora soluzioni nella visualizzazione di progettazione della pagina.

[![aggiungere il controllo utente SectionLevelTutorialListing. ascx a default. aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Figura 2**: aggiungere il controllo utente `SectionLevelTutorialListing.ascx` al `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))

Aggiungere infine le pagine come voci al file di `Web.sitemap`. In particolare, aggiungere il markup seguente dopo i report master/dettagli con il `<siteMapNode>`DataList e Repeater:

[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

Dopo avere aggiornato `Web.sitemap`, è possibile visualizzare il sito Web delle esercitazioni tramite un browser. Il menu a sinistra include ora gli elementi per le esercitazioni di modifica ed eliminazione di DataList.

![La mappa del sito include ora le voci per le esercitazioni per la modifica e l'eliminazione di DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Figura 3**: la mappa del sito include ora le voci per le esercitazioni di modifica ed eliminazione di DataList

## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Passaggio 2: analisi delle tecniche per l'aggiornamento e l'eliminazione di dati

La modifica e l'eliminazione dei dati con GridView è così semplice perché, sotto le quinte, GridView e ObjectDataSource funzionano in concerto. Come illustrato nell'esercitazione [esaminare gli eventi associati all'inserimento, all'aggiornamento e all'eliminazione](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) , quando si fa clic su un pulsante di aggiornamento di una riga, GridView assegna automaticamente i campi che utilizzano l'associazione dati bidirezionale alla raccolta `UpdateParameters` del relativo ObjectDataSource, quindi richiama il metodo objectdatasource s `Update()`.

Purtroppo, DataList non fornisce alcuna funzionalità incorporata. È responsabilità dell'utente assicurarsi che i valori dell'utente siano assegnati ai parametri di ObjectDataSource e che venga chiamato il relativo metodo `Update()`. Per aiutarci in questo sforzo, DataList fornisce le proprietà e gli eventi seguenti:

- Quando si aggiorna o si elimina **la [Proprietà`DataKeyField`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  , è necessario essere in grado di identificare in modo univoco ogni elemento del DataList. Impostare questa proprietà sul campo chiave primaria dei dati visualizzati. In questo modo la [raccolta`DataKeys`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) di DataList viene popolata con il valore di `DataKeyField` specificato per ogni elemento DataList.
- **L' [evento`EditCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  viene generato quando si fa clic su un pulsante, un LinkButton o un ImageButton la cui proprietà `CommandName` è impostata su modifica.
- **L' [evento`CancelCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  viene generato quando si fa clic su un pulsante, un LinkButton o un ImageButton la cui proprietà `CommandName` è impostata su Annulla.
- **L' [evento`UpdateCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  viene generato quando si fa clic su un pulsante, un LinkButton o un ImageButton la cui proprietà `CommandName` è impostata su Update.
- **L' [evento`DeleteCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  viene generato quando si fa clic su un pulsante, un LinkButton o un ImageButton la cui proprietà `CommandName` è impostata su Delete.

Utilizzando queste proprietà ed eventi, sono disponibili quattro approcci che è possibile utilizzare per aggiornare ed eliminare dati da DataList:

1. **Utilizzando le tecniche ASP.NET 1. x** , il DataList esisteva prima di ASP.NET 2,0 ed ObjectDataSource ed è stato in grado di aggiornare ed eliminare completamente i dati attraverso i mezzi programmatici. Questa tecnica abbandona completamente ObjectDataSource ed è necessario associare i dati al DataList direttamente dal livello della logica di business, sia per il recupero dei dati da visualizzare sia per l'aggiornamento o l'eliminazione di un record.
2. Se si **utilizza un singolo controllo ObjectDataSource nella pagina per la selezione, l'aggiornamento e l'eliminazione** di, mentre in DataList mancano le funzionalità di modifica e eliminazione intrinseche di GridView, non è possibile aggiungerle a se stessi. Con questo approccio, viene usato un oggetto ObjectDataSource come negli esempi di GridView, ma è necessario creare un gestore eventi per l'evento `UpdateCommand` di DataList in cui si impostano i parametri di ObjectDataSource e si chiama il relativo metodo `Update()`.
3. **Utilizzando un controllo ObjectDataSource per la selezione, ma l'aggiornamento e l'eliminazione direttamente in base a BLL** quando si usa l'opzione 2, è necessario scrivere un po' di codice nell'evento `UpdateCommand`, assegnando i valori dei parametri e così via. È invece possibile usare ObjectDataSource per la selezione, ma eseguire le chiamate di aggiornamento ed eliminazione direttamente sul BLL (ad esempio con l'opzione 1). Nel mio parere, l'aggiornamento dei dati tramite l'interazione diretta con il BLL comporta un codice più leggibile rispetto all'assegnazione del `UpdateParameters` di ObjectDataSource e alla chiamata del relativo metodo `Update()`.
4. **Utilizzando il metodo dichiarativo attraverso più ObjectDataSource** , i tre approcci precedenti richiedono un po' di codice. Se si preferisce utilizzare la sintassi dichiarativa più possibile, un'opzione finale consiste nell'includere più ObjectDataSource nella pagina. Il primo ObjectDataSource recupera i dati dal BLL e li associa a DataList. Per l'aggiornamento, viene aggiunto un altro ObjectDataSource, ma aggiunto direttamente all'interno del `EditItemTemplate`di DataList. Per includere l'eliminazione del supporto, è necessario un altro ObjectDataSource nell'`ItemTemplate`. Con questo approccio, questi oggetti ObjectDataSource incorporati usano `ControlParameters` per associare in modo dichiarativo i parametri di ObjectDataSource ai controlli di input utente, anziché doverli specificare a livello di codice nel gestore dell'evento DataList `UpdateCommand`. Questo approccio richiede ancora un po' di codice, ma è necessario chiamare il comando ObjectDataSource s incorporato `Update()` o `Delete()`, ma è necessario molto meno rispetto agli altri tre approcci. Il lato negativo è che i più ObjectDataSource eseguono un disordine della pagina, detratti dalla leggibilità complessiva.

Se forzato a usare solo uno di questi approcci, scelgo l'opzione 1 perché fornisce la massima flessibilità e perché il DataList è stato originariamente progettato per adattarsi a questo modello. Mentre DataList è stato esteso per funzionare con i controlli origine dati ASP.NET 2,0, non dispone di tutti i punti di estendibilità o le funzionalità dei controlli Web ASP.NET 2,0 Data Official (GridView, DetailsView e FormView). Tuttavia, le opzioni da 2 a 4 non sono di alcun merito.

Questa e la futura esercitazione per la modifica e l'eliminazione utilizzeranno un ObjectDataSource per recuperare i dati da visualizzare e indirizzare le chiamate a BLL per aggiornare ed eliminare i dati (opzione 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Passaggio 3: aggiunta del DataList e configurazione di ObjectDataSource

In questa esercitazione verrà creato un DataList che elenca le informazioni sul prodotto e, per ogni prodotto, consente all'utente di modificare il nome e il prezzo ed eliminare completamente il prodotto. In particolare, si recupereranno i record da visualizzare mediante ObjectDataSource, ma si eseguiranno le azioni di aggiornamento ed eliminazione tramite l'interazione diretta con il BLL. Prima di preoccuparsi di implementare le funzionalità di modifica ed eliminazione per DataList, è necessario innanzitutto ottenere la pagina per visualizzare i prodotti in un'interfaccia di sola lettura. Poiché è stata esaminata la procedura descritta nelle esercitazioni precedenti, I passaggi verranno eseguiti rapidamente.

Per iniziare, aprire la pagina `Basics.aspx` nella cartella `EditDeleteDataList` e, dalla visualizzazione progettazione, aggiungere un DataList alla pagina. Quindi, dallo smart tag di DataList, creare un nuovo ObjectDataSource. Poiché si utilizzano i dati del prodotto, configurarlo per l'utilizzo della classe `ProductsBLL`. Per recuperare *tutti i* prodotti, scegliere il metodo `GetProducts()` nella scheda Seleziona.

[![configurare ObjectDataSource per l'utilizzo della classe ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Figura 4**: configurare ObjectDataSource per l'uso della classe `ProductsBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))

[![restituire le informazioni sul prodotto utilizzando il metodo GetProducts ()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Figura 5**: restituire le informazioni sul prodotto utilizzando il metodo `GetProducts()` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))

DataList, come GridView, non è progettato per l'inserimento di nuovi dati; Selezionare quindi l'opzione (nessuno) nell'elenco a discesa nella scheda Inserisci. È inoltre possibile scegliere (nessuno) per le schede Aggiorna ed Elimina poiché gli aggiornamenti e le eliminazioni verranno eseguiti a livello di codice tramite il BLL.

[![verificare che gli elenchi a discesa nelle schede di inserimento, aggiornamento ed eliminazione di ObjectDataSource siano impostati su (nessuno)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Figura 6**: verificare che gli elenchi a discesa nelle schede di inserimento, aggiornamento ed eliminazione di ObjectDataSource s siano impostati su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))

Dopo aver configurato ObjectDataSource, fare clic su fine e tornare alla finestra di progettazione. Come illustrato negli esempi precedenti, quando si completa la configurazione di ObjectDataSource, Visual Studio crea automaticamente un `ItemTemplate` per il DropDownList, visualizzando ogni campo dati. Sostituire questa `ItemTemplate` con una che visualizza solo il nome e il prezzo del prodotto. Inoltre, impostare la proprietà `RepeatColumns` su 2.

> [!NOTE]
> Come illustrato nella *Panoramica dell'esercitazione sull'inserimento, l'aggiornamento e l'eliminazione dei dati* , quando si modificano i dati mediante ObjectDataSource, l'architettura richiede la rimozione della proprietà `OldValuesParameterFormatString` dal markup dichiarativo di ObjectDataSource (o la reimpostazione del valore predefinito `{0}`). In questa esercitazione, tuttavia, viene utilizzato ObjectDataSource solo per il recupero dei dati. Pertanto, non è necessario modificare il valore della proprietà `OldValuesParameterFormatString` di ObjectDataSource (anche se non è stato danneggiato).

Dopo aver sostituito il `ItemTemplate` DataList predefinito con uno personalizzato, il markup dichiarativo nella pagina dovrebbe essere simile al seguente:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Per visualizzare lo stato di avanzamento, è possibile usare un browser. Come illustrato nella figura 7, DataList Visualizza il nome del prodotto e il prezzo unitario per ogni prodotto in due colonne.

[![i nomi dei prodotti e i prezzi vengono visualizzati in un DataList a due colonne](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Figura 7**: i nomi dei prodotti e i prezzi vengono visualizzati in un DataList a due colonne ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))

> [!NOTE]
> DataList dispone di una serie di proprietà necessarie per il processo di aggiornamento ed eliminazione e tali valori vengono archiviati in stato di visualizzazione. Pertanto, quando si compila un DataList che supporta la modifica o l'eliminazione di dati, è essenziale che lo stato di visualizzazione di DataList sia abilitato.  
>   
> Il lettore astuto potrebbe ricordare che è stato possibile disabilitare lo stato di visualizzazione durante la creazione di GridView, DetailsView e FormView modificabili. Ciò è dovuto al fatto che i controlli Web ASP.NET 2,0 possono includere lo stato del *controllo*, che è stato reso permanente nei postback come lo stato di visualizzazione, ma è considerato essenziale.

La disabilitazione dello stato di visualizzazione in GridView omette semplicemente le informazioni di stato banali, ma mantiene lo stato del controllo (che include lo stato necessario per la modifica e l'eliminazione). DataList, creato nell'intervallo di tempo ASP.NET 1. x, non utilizza lo stato del controllo e pertanto deve disporre dello stato di visualizzazione abilitato. Vedere lo stato del [controllo e lo stato di visualizzazione](https://msdn.microsoft.com/library/1whwt1k7.aspx) per ulteriori informazioni sullo scopo dello stato del controllo e su come si differenzia dallo stato di visualizzazione.

## <a name="step-4-adding-an-editing-user-interface"></a>Passaggio 4: aggiunta di un'interfaccia utente di modifica

Il controllo GridView è costituito da una raccolta di campi (BoundFields, CheckBoxFields, TemplateFields e così via). Questi campi possono modificare il markup di cui è stato eseguito il rendering in base alla modalità. In modalità di sola lettura, ad esempio, un BoundField Visualizza il relativo valore del campo dati come testo; in modalità di modifica, viene eseguito il rendering di un controllo Web TextBox la cui proprietà `Text` viene assegnato al valore del campo dati.

DataList, invece, esegue il rendering degli elementi utilizzando i modelli. Il rendering degli elementi di sola lettura viene eseguito utilizzando il `ItemTemplate` mentre gli elementi in modalità di modifica vengono sottoposti a rendering tramite l'`EditItemTemplate`. A questo punto, il DataList ha solo un `ItemTemplate`. Per supportare la funzionalità di modifica a livello di elemento, è necessario aggiungere un `EditItemTemplate` contenente il markup da visualizzare per l'elemento modificabile. Per questa esercitazione verranno usati i controlli Web TextBox per modificare il nome e il prezzo unitario del prodotto.

Il `EditItemTemplate` può essere creato in modo dichiarativo o tramite la finestra di progettazione (selezionando l'opzione modifica modelli dallo smart tag di DataList). Per utilizzare l'opzione modifica modelli, fare prima clic sul collegamento modifica modelli nello smart tag, quindi selezionare l'elemento `EditItemTemplate` dall'elenco a discesa.

[![scegliere di utilizzare DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Figura 8**: scegliere di utilizzare il `EditItemTemplate` DataList ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))

Digitare quindi nome prodotto: e Price: e trascinare due controlli TextBox dalla casella degli strumenti nell'interfaccia `EditItemTemplate` della finestra di progettazione. Impostare le caselle di testo `ID` proprietà su `ProductName` e `UnitPrice`.

[![aggiungere una casella di testo per il nome e il prezzo del prodotto](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Figura 9**: aggiungere una casella di testo per il nome del prodotto e il prezzo ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))

È necessario associare i valori del campo dei dati di prodotto corrispondenti alle proprietà `Text` delle due caselle di testo. Dagli smart tag caselle di testo, fare clic sul collegamento Modifica DataBindings, quindi associare il campo dati appropriato alla proprietà `Text`, come illustrato nella figura 10.

> [!NOTE]
> Quando si associa il campo dati `UnitPrice` al campo `Text` della casella di testo Price, è possibile formattarlo come valore di valuta (`{0:C}`), un numero generale (`{0:N}`) o lasciarlo non formattato.

![Associare i campi dati ProductName e PrezzoUnitario alle proprietà Text delle caselle di testo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Figura 10**: associare i campi dati `ProductName` e `UnitPrice` alle proprietà `Text` delle caselle di testo

Si noti che la finestra di dialogo Modifica DataBindings nella figura 10 *non* include la casella di controllo DataBinding bidirezionale presente durante la modifica di un TemplateField in GridView o DetailsView o un modello in FormView. La funzionalità di associazione dati bidirezionale ha consentito l'assegnazione automatica del valore immesso nel controllo Web di input ai `InsertParameters` o `UpdateParameters` di ObjectDataSource corrispondenti durante l'inserimento o l'aggiornamento dei dati. DataList non supporta l'associazione dati bidirezionale come si vedrà più avanti in questa esercitazione, dopo che l'utente ha apportato le modifiche ed è pronto per l'aggiornamento dei dati, sarà necessario accedere a livello di codice a queste caselle di testo `Text` proprietà e passare i rispettivi valori al metodo `UpdateProduct` appropriato nella classe `ProductsBLL`.

Infine, è necessario aggiungere i pulsanti Aggiorna e Annulla alla `EditItemTemplate`. Come è stato illustrato in [Master/Detail utilizzando un elenco puntato di record master con un'esercitazione su DataList di dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) , quando si fa clic su un pulsante, un LinkButton o un ImageButton la cui proprietà `CommandName` è impostata dall'interno di un oggetto Repeater o DataList, viene generato l'evento Repeater o DataList `ItemCommand`. Per DataList, se la proprietà `CommandName` è impostata su un determinato valore, potrebbe essere generato anche un altro evento. I valori speciali della proprietà `CommandName` includono, tra gli altri:

- Annulla genera l'evento `CancelCommand`
- Modifica genera l'evento `EditCommand`
- L'aggiornamento genera l'evento `UpdateCommand`

Tenere presente che questi eventi vengono generati *in aggiunta all'* evento `ItemCommand`.

Aggiungere all'`EditItemTemplate` controlli Web con due pulsanti, uno con `CommandName` impostato su Aggiorna e l'altro impostato su Annulla. Dopo aver aggiunto i controlli Web di questi due pulsanti, la finestra di progettazione dovrebbe avere un aspetto simile al seguente:

[![aggiungere i pulsanti Aggiorna e Annulla a EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Figura 11**: aggiungere pulsanti Aggiorna e annulla alla `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))

Con il `EditItemTemplate` completare il markup dichiarativo di DataList sarà simile al seguente:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Passaggio 5: aggiunta del plumbing per passare alla modalità di modifica

A questo punto il DataList dispone di un'interfaccia di modifica definita tramite la relativa `EditItemTemplate`; Tuttavia, attualmente non esiste alcun modo per un utente che visita la pagina per indicare che desidera modificare le informazioni di un prodotto. È necessario aggiungere un pulsante modifica a ogni prodotto che, quando selezionato, esegue il rendering dell'elemento DataList in modalità di modifica. Per iniziare, aggiungere un pulsante modifica alla `ItemTemplate`, tramite la finestra di progettazione o in modo dichiarativo. Assicurarsi di impostare il pulsante modifica `CommandName` proprietà da modificare.

Una volta aggiunto questo pulsante di modifica, è possibile visualizzare la pagina tramite un browser. Con questa aggiunta, ogni listato di prodotti deve includere un pulsante di modifica.

[![aggiungere i pulsanti Aggiorna e Annulla a EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Figura 12**: aggiungere pulsanti Aggiorna e annulla alla `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))

Se si fa clic sul pulsante, viene generato un postback, ma il prodotto *non* viene inserito in modalità di modifica. Per rendere modificabile il prodotto, è necessario:

1. Impostare la [proprietà`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) di DataList sull'indice della `DataListItem` di cui è stato appena fatto clic sul pulsante modifica.
2. Riassociare i dati al DataList. Quando si esegue di nuovo il rendering del DataList, il `DataListItem` la cui `ItemIndex` corrisponde al `EditItemIndex` di DataList verrà eseguito il rendering utilizzando la `EditItemTemplate`.

Poiché l'evento `EditCommand` di DataList viene generato quando si fa clic sul pulsante modifica, creare un gestore dell'evento `EditCommand` con il codice seguente:

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

Il gestore dell'evento `EditCommand` viene passato in un oggetto di tipo `DataListCommandEventArgs` come secondo parametro di input, che include un riferimento al `DataListItem` di cui è stato fatto clic sul pulsante modifica (`e.Item`). Il gestore eventi imposta prima di tutto il `EditItemIndex` di DataList sul `ItemIndex` del `DataListItem` modificabile e quindi riassocia i dati al DataList chiamando il metodo `DataBind()` di DataList.

Dopo l'aggiunta di questo gestore eventi, rivedere la pagina in un browser. Facendo clic sul pulsante modifica, il prodotto selezionato viene reso modificabile (vedere la figura 13).

[![fare clic sul pulsante modifica rende il prodotto modificabile](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Figura 13**: facendo clic sul pulsante modifica il prodotto diventa modificabile ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))

## <a name="step-6-saving-the-user-s-changes"></a>Passaggio 6: salvataggio delle modifiche apportate dall'utente

Il clic sui pulsanti Aggiorna o Annulla del prodotto modificato non esegue alcuna operazione in questa fase; per aggiungere questa funzionalità, è necessario creare gestori eventi per gli eventi `UpdateCommand` e `CancelCommand` di DataList. Per iniziare, creare il gestore dell'evento `CancelCommand`, che verrà eseguito quando si fa clic sul pulsante Annulla del prodotto modificato ed è necessario che venga restituito il DataList allo stato di pre-modifica.

Per fare in modo che DataList esegua il rendering di tutti gli elementi in modalità di sola lettura, è necessario:

1. Impostare la [proprietà`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) di DataList sull'indice di un indice `DataListItem` non esistente. `-1` è una scelta sicura, poiché gli indici `DataListItem` iniziano da `0`.
2. Riassociare i dati al DataList. Poiché nessuna `DataListItem` `ItemIndex` es corrisponde al `EditItemIndex`di DataList, l'intero DataList verrà sottoposto a rendering in modalità di sola lettura.

Questi passaggi possono essere eseguiti con il seguente codice del gestore eventi:

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

Con questa aggiunta, facendo clic sul pulsante Annulla viene restituito il DataList allo stato di pre-modifica.

L'ultimo gestore eventi che è necessario completare è il gestore dell'evento `UpdateCommand`. Questo gestore eventi deve:

1. Accedere a livello di codice al nome del prodotto e al prezzo immessi dall'utente, oltre che `ProductID`del prodotto modificato.
2. Avviare il processo di aggiornamento chiamando l'overload `UpdateProduct` appropriato nella classe `ProductsBLL`.
3. Impostare la [proprietà`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) di DataList sull'indice di un indice `DataListItem` non esistente. `-1` è una scelta sicura, poiché gli indici `DataListItem` iniziano da `0`.
4. Riassociare i dati al DataList. Poiché nessuna `DataListItem` `ItemIndex` es corrisponde al `EditItemIndex`di DataList, l'intero DataList verrà sottoposto a rendering in modalità di sola lettura.

I passaggi 1 e 2 sono responsabili del salvataggio delle modifiche dell'utente. i passaggi 3 e 4 restituiscono il DataList allo stato di pre-modifica dopo che le modifiche sono state salvate e sono identiche ai passaggi eseguiti nel gestore dell'evento `CancelCommand`.

Per ottenere il nome e il prezzo del prodotto aggiornati, è necessario usare il metodo `FindControl` per fare riferimento a a livello di codice ai controlli Web della casella di testo all'interno del `EditItemTemplate`. È necessario ottenere anche il valore `ProductID` del prodotto modificato. Quando inizialmente il ObjectDataSource è stato associato a DataList, Visual Studio ha assegnato la proprietà `DataKeyField` di DataList al valore di chiave primaria dall'origine dati (`ProductID`). Questo valore può quindi essere recuperato dalla raccolta di `DataKeys` di DataList. Assicurarsi che la proprietà `DataKeyField` sia effettivamente impostata su `ProductID`.

Il codice seguente implementa i quattro passaggi:

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

Il gestore eventi inizia leggendo il `ProductID` del prodotto modificato dalla raccolta di `DataKeys`. A questo punto, alle due caselle di testo nel `EditItemTemplate` viene fatto riferimento e le relative proprietà `Text` archiviate in variabili locali, `productNameValue` e `unitPriceValue`. Viene usato il metodo `Decimal.Parse()` per leggere il valore dalla casella di testo `UnitPrice` in modo che, se il valore immesso ha un simbolo di valuta, può comunque essere convertito correttamente in un valore di `Decimal`.

> [!NOTE]
> I valori delle caselle di testo `ProductName` e `UnitPrice` vengono assegnati solo alle variabili productNameValue e unitPriceValue se per le proprietà Text di TextBox è specificato un valore. In caso contrario, viene utilizzato un valore di `Nothing` per le variabili, che ha l'effetto di aggiornare i dati con un valore di `NULL` del database. In altre condizioni, il codice considera la conversione di stringhe vuote in valori di `NULL` di database, ovvero il comportamento predefinito dell'interfaccia di modifica nei controlli GridView, DetailsView e FormView.

Dopo la lettura dei valori, viene chiamato il metodo `ProductsBLL` Class s `UpdateProduct`, passando il nome, il prezzo e la `ProductID`del prodotto. Il gestore eventi viene completato restituendo il DataList allo stato di pre-modifica utilizzando la stessa logica del gestore dell'evento `CancelCommand`.

Con i gestori di eventi `EditCommand`, `CancelCommand`e `UpdateCommand` completati, un visitatore può modificare il nome e il prezzo di un prodotto. Le figure 14-16 mostrano il flusso di lavoro di modifica in azione.

[![quando si visita la pagina per la prima volta, tutti i prodotti sono in modalità di sola lettura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Figura 14**: quando si visita la pagina per la prima volta, tutti i prodotti sono in modalità di sola lettura ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))

[![aggiornare il nome o il prezzo di un prodotto, fare clic sul pulsante modifica](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Figura 15**: per aggiornare il nome o il prezzo di un prodotto, fare clic sul pulsante modifica ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))

[![dopo la modifica del valore, fare clic su Aggiorna per tornare alla modalità di sola lettura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Figura 16**: dopo aver modificato il valore, fare clic su Aggiorna per tornare alla modalità di sola lettura ([fare clic per visualizzare l'immagine con dimensioni complete](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))

## <a name="step-7-adding-delete-capabilities"></a>Passaggio 7: aggiunta di funzionalità di eliminazione

I passaggi per l'aggiunta di funzionalità di eliminazione a un DataList sono simili a quelli per l'aggiunta di funzionalità di modifica. In breve, è necessario aggiungere un pulsante Delete per la `ItemTemplate` che, quando si fa clic su di esso:

1. Legge il prodotto corrispondente `ProductID` tramite la raccolta di `DataKeys`.
2. Esegue l'eliminazione chiamando il metodo `ProductsBLL` Class s `DeleteProduct`.
3. Riassocia i dati al DataList.

Per iniziare, è necessario aggiungere un pulsante Elimina all'`ItemTemplate`.

Quando si fa clic su un pulsante il cui `CommandName` è modifica, aggiorna o Annulla, genera l'evento `ItemCommand` di DataList insieme a un evento aggiuntivo, ad esempio quando si utilizza modifica l'evento `EditCommand` viene generato anche. Analogamente, qualsiasi pulsante, LinkButton o ImageButton nel DataList la cui proprietà `CommandName` è impostata su Delete determina l'attivazione dell'evento `DeleteCommand` (insieme alla `ItemCommand`).

Aggiungere un pulsante Elimina accanto al pulsante modifica nella `ItemTemplate`, impostando la relativa proprietà `CommandName` su Delete. Dopo avere aggiunto questo controllo Button, il controllo DataList `ItemTemplate` sintassi dichiarativa dovrebbe essere simile al seguente:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

Successivamente, creare un gestore eventi per l'evento `DeleteCommand` di DataList, usando il codice seguente:

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

Se si fa clic sul pulsante Elimina, viene generato un postback e viene attivato l'evento DataList s `DeleteCommand`. Nel gestore eventi, il valore `ProductID` del prodotto selezionato è accessibile dalla raccolta di `DataKeys`. Il prodotto viene quindi eliminato chiamando il metodo `ProductsBLL` Class s `DeleteProduct`.

Dopo l'eliminazione del prodotto, è importante riassociare i dati a DataList (`DataList1.DataBind()`); in caso contrario, DataList continuerà a mostrare il prodotto appena eliminato.

## <a name="summary"></a>Riepilogo

Mentre DataList non dispone del punto e fa clic su modifica ed Elimina il supporto di GridView, con un breve frammento di codice può essere migliorato per includere queste funzionalità. In questa esercitazione è stato illustrato come creare un elenco a due colonne di prodotti che potrebbero essere eliminati e di cui è possibile modificare il nome e il prezzo. L'aggiunta del supporto per la modifica e l'eliminazione è una cosa che include i controlli Web appropriati nell'`ItemTemplate` e `EditItemTemplate`, la creazione dei gestori eventi corrispondenti, la lettura dei valori immessi dall'utente e della chiave primaria e l'interazione con il livello della logica di business.

Sebbene siano state aggiunte funzionalità di base di modifica ed eliminazione a DataList, non sono disponibili funzionalità più avanzate. Ad esempio, non è presente alcuna convalida dei campi di input: se un utente immette un prezzo troppo costoso, un'eccezione verrà generata da `Decimal.Parse` durante il tentativo di conversione troppo costosa in una `Decimal`. Analogamente, se si verifica un problema durante l'aggiornamento dei dati alla logica di business o ai livelli di accesso ai dati, l'utente verrà visualizzato con la schermata di errore standard. Senza alcuna conferma sul pulsante Elimina, l'eliminazione accidentale di un prodotto è troppo probabile.

Nelle esercitazioni future si vedrà come migliorare l'esperienza utente di modifica.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Zack Jones, Ken Pespisa e Randy Schmidt. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](customizing-the-datalist-s-editing-interface-cs.md)
> [Successivo](performing-batch-updates-vb.md)
