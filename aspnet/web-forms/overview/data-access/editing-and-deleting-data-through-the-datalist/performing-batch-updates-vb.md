---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Esecuzione di aggiornamenti batch (VB) | Microsoft Docs
author: rick-anderson
description: Informazioni su come creare un oggetto DataList completamente modificabile in cui tutti gli elementi sono in modalità di modifica e i cui valori possono essere salvati facendo clic sul pulsante ' Aggiorna tutto ' nel...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: e54c9fc12da278492b54164cf657eea142a3dae6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78610698"
---
# <a name="performing-batch-updates-vb"></a>Esecuzione di aggiornamenti batch (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) o [scaricare il file PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Informazioni su come creare un oggetto DataList completamente modificabile in cui tutti gli elementi sono in modalità di modifica e i cui valori possono essere salvati facendo clic sul pulsante "Aggiorna tutto" nella pagina.

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) è stato esaminato come creare un DataList a livello di elemento. Come il GridView modificabile standard, ogni elemento in DataList include un pulsante di modifica che, quando selezionato, rende l'elemento modificabile. Sebbene questa modifica a livello di elemento funzioni correttamente per i dati che vengono aggiornati solo occasionalmente, alcuni scenari di caso d'uso richiedono che l'utente modifichi molti record. Se un utente deve modificare decine di record ed è forzato a fare clic su modifica, apportare le modifiche e fare clic su Aggiorna per ciascuna di esse, la quantità di clic può ostacolare la produttività. In tali situazioni, un'opzione migliore consiste nel fornire un oggetto DataList completamente modificabile, uno in cui *tutti* gli elementi sono in modalità di modifica e i cui valori possono essere modificati facendo clic sul pulsante Aggiorna tutto nella pagina (vedere la figura 1).

[![ogni elemento in un oggetto DataList completamente modificabile può essere modificato](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Figura 1**: è possibile modificare ogni elemento in un oggetto DataList completamente modificabile ([fare clic per visualizzare l'immagine con dimensioni complete](performing-batch-updates-vb/_static/image3.png))

In questa esercitazione verrà illustrato come consentire agli utenti di aggiornare le informazioni sull'indirizzo del fornitore usando un oggetto DataList completamente modificabile.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Passaggio 1: creare l'interfaccia utente modificabile in DataList s ItemTemplate

Nell'esercitazione precedente, in cui è stato creato un DataList standard, modificabile a livello di elemento, sono stati usati due modelli:

- `ItemTemplate` conteneva l'interfaccia utente di sola lettura (i controlli Web etichetta per visualizzare il nome e il prezzo di ogni prodotto).
- `EditItemTemplate` conteneva l'interfaccia utente della modalità di modifica, ovvero i due controlli Web TextBox.

La proprietà `EditItemIndex` di DataList impone il rendering di `DataListItem`, se presente, utilizzando il `EditItemTemplate`. In particolare, il `DataListItem` il cui valore `ItemIndex` corrisponde alla proprietà `EditItemIndex` di DataList viene eseguito il rendering utilizzando il `EditItemTemplate`. Questo modello funziona bene quando è possibile modificare un solo elemento alla volta, ma si distingue quando si crea un DataList completamente modificabile.

Per un DataList completamente modificabile, è necessario che *tutti* i `DataListItem` eseguano il rendering usando l'interfaccia modificabile. Il modo più semplice per eseguire questa operazione consiste nel definire l'interfaccia modificabile nel `ItemTemplate`. Per modificare le informazioni sull'indirizzo dei fornitori, l'interfaccia modificabile contiene il nome del fornitore come testo e quindi le caselle di testo per i valori Address, City e Country.

Per iniziare, aprire la pagina `BatchUpdate.aspx`, aggiungere un controllo DataList e impostare la relativa proprietà `ID` su `Suppliers`. Dallo smart tag di DataList, scegliere di aggiungere un nuovo controllo ObjectDataSource denominato `SuppliersDataSource`.

[![creare un nuovo ObjectDataSource denominato SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Figura 2**: creare un nuovo ObjectDataSource denominato `SuppliersDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](performing-batch-updates-vb/_static/image6.png))

Configurare ObjectDataSource per recuperare i dati utilizzando il metodo `SuppliersBLL` Class s `GetSuppliers()` (vedere la figura 3). Come per l'esercitazione precedente, invece di aggiornare le informazioni del fornitore tramite ObjectDataSource, si funzionerà direttamente con il livello di logica di business. Quindi, impostare l'elenco a discesa su (nessuno) nella scheda aggiornamento (vedere la figura 4).

[![recuperare le informazioni sui fornitori utilizzando il metodo getsuppliers ()](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Figura 3**: recuperare le informazioni sul fornitore usando il metodo `GetSuppliers()` ([fare clic per visualizzare l'immagine con dimensioni complete](performing-batch-updates-vb/_static/image9.png))

[![impostare l'elenco a discesa (nessuno) nella scheda aggiornamento](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Figura 4**: impostare l'elenco a discesa su (nessuno) nella scheda aggiornamento ([fare clic per visualizzare l'immagine con dimensioni complete](performing-batch-updates-vb/_static/image12.png))

Dopo aver completato la procedura guidata, Visual Studio genera automaticamente il `ItemTemplate` di DataList per visualizzare ogni campo dati restituito dall'origine dati in un controllo Web Label. È necessario modificare il modello in modo che fornisca invece l'interfaccia di modifica. Il `ItemTemplate` può essere personalizzato tramite la finestra di progettazione utilizzando l'opzione modifica modelli dello smart tag di DataList o direttamente tramite la sintassi dichiarativa.

Creare un'interfaccia di modifica che visualizzi il nome del fornitore come testo, ma includa le caselle di testo per i valori relativi a indirizzo, città e paese del fornitore. Dopo aver apportato queste modifiche, la sintassi dichiarativa della pagina deve essere simile alla seguente:

[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Come per l'esercitazione precedente, per DataList in questa esercitazione deve essere abilitato lo stato di visualizzazione.

Nella `ItemTemplate` I m usano due nuove classi CSS, `SupplierPropertyLabel` e `SupplierPropertyValue`, che sono state aggiunte alla classe `Styles.css` e configurate per usare le stesse impostazioni di stile delle classi CSS `ProductPropertyLabel` e `ProductPropertyValue`.

[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Dopo avere apportato queste modifiche, visitare questa pagina tramite un browser. Come illustrato nella figura 5, ogni elemento DataList Visualizza il nome del fornitore come testo e usa le caselle di testo per visualizzare indirizzo, città e paese.

[![ogni fornitore nel DataList è modificabile](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Figura 5**: ogni fornitore del DataList è modificabile ([fare clic per visualizzare l'immagine con dimensioni complete](performing-batch-updates-vb/_static/image15.png))

## <a name="step-2-adding-an-update-all-button"></a>Passaggio 2: aggiunta di un pulsante Aggiorna tutto

Mentre ogni fornitore nella figura 5 ha i campi indirizzo, città e paese visualizzati in una casella di testo, attualmente non è disponibile alcun pulsante Aggiorna. Anziché avere un pulsante di aggiornamento per ogni elemento, con gli elenchi di dati completamente modificabili è in genere disponibile un singolo pulsante Aggiorna tutto nella pagina che, quando selezionato, aggiorna *tutti* i record nel DataList. Per questa esercitazione, è possibile aggiungere due pulsanti Aggiorna tutti: uno nella parte superiore della pagina e uno nella parte inferiore (anche se fare clic su uno dei pulsanti avrà lo stesso effetto).

Per iniziare, aggiungere un controllo Web Button sopra DataList e impostare la relativa proprietà `ID` su `UpdateAll1`. Successivamente, aggiungere il secondo controllo Web Button sotto DataList, impostando il `ID` su `UpdateAll2`. Impostare le proprietà `Text` per i due pulsanti per aggiornarli tutti. Infine, creare i gestori eventi per entrambi i pulsanti `Click` eventi. Anziché duplicare la logica di aggiornamento in ogni gestore eventi, è possibile effettuare il refactoring di tale logica a un terzo metodo, `UpdateAllSupplierAddresses`, che i gestori eventi richiamano semplicemente questo terzo metodo.

[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

Nella figura 6 viene illustrata la pagina dopo l'aggiunta dei pulsanti Aggiorna tutti.

[![due pulsanti Aggiorna tutti sono stati aggiunti alla pagina](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Figura 6**: due pulsanti Aggiorna tutti sono stati aggiunti alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](performing-batch-updates-vb/_static/image18.png))

## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Passaggio 3: aggiornamento di tutte le informazioni sull'indirizzo dei fornitori

Con tutti gli elementi di DataList che visualizzano l'interfaccia di modifica e con l'aggiunta dei pulsanti Aggiorna tutti, tutto ciò che rimane sta scrivendo il codice per eseguire l'aggiornamento del batch. In particolare, è necessario eseguire il ciclo degli elementi di DataList e chiamare il metodo `SuppliersBLL` Class s `UpdateSupplierAddress` per ciascuno di essi.

La raccolta di istanze di `DataListItem` per cui è possibile accedere al DataList tramite la [proprietà`Items`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx)di DataList. Con un riferimento a un `DataListItem`, è possibile recuperare i `SupplierID` corrispondenti dalla raccolta `DataKeys` e fare riferimento a livello di codice ai controlli Web della casella di testo all'interno del `ItemTemplate` come illustrato nel codice seguente:

[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Quando l'utente fa clic su uno dei pulsanti Aggiorna tutti, il metodo `UpdateAllSupplierAddresses` scorre ogni `DataListItem` nel `Suppliers` DataList e chiama il metodo `UpdateSupplierAddress` della classe `SuppliersBLL`, passando i valori corrispondenti. Un valore non immesso per le passate Address, City o Country è un valore di `Nothing` `UpdateSupplierAddress` (anziché una stringa vuota), che restituisce un `NULL` di database per i campi del record sottostante.

> [!NOTE]
> Come miglioramento, potrebbe essere necessario aggiungere un controllo Web etichetta stato alla pagina che fornisce un messaggio di conferma dopo l'esecuzione dell'aggiornamento batch.

## <a name="updating-only-those-addresses-that-have-been-modified"></a>Aggiornamento solo degli indirizzi modificati

L'algoritmo di aggiornamento batch usato per questa esercitazione chiama il metodo `UpdateSupplierAddress` per *ogni* fornitore nel DataList, indipendentemente dal fatto che le informazioni relative all'indirizzo siano state modificate. Sebbene questi aggiornamenti ciechi non siano in genere un problema di prestazioni, possono causare record superflui se si ricontrollano le modifiche apportate alla tabella di database. Se, ad esempio, si utilizzano i trigger per registrare tutti `UPDATE` nella tabella `Suppliers` in una tabella di controllo, ogni volta che un utente fa clic sul pulsante Aggiorna tutto, viene creato un nuovo record di controllo per ogni fornitore del sistema, indipendentemente dal fatto che l'utente abbia apportato modifiche.

Le classi DataTable e DataAdapter di ADO.NET sono progettate per supportare gli aggiornamenti in batch in cui solo i record modificati, eliminati e nuovi generano qualsiasi comunicazione del database. Ogni riga nell'oggetto DataTable dispone di una [proprietà`RowState`](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) che indica se la riga è stata aggiunta alla DataTable, ne è stata eliminata, modificata o rimane invariata. Quando un DataTable viene inizialmente popolato, tutte le righe vengono contrassegnate come invariate. Se si modifica il valore di una delle colonne della riga, la riga viene contrassegnata come modificata.

Nella classe `SuppliersBLL` si aggiornano le informazioni sull'indirizzo del fornitore specificato leggendo prima di tutto il record del fornitore singolo in un `SuppliersDataTable` e quindi si impostano i valori di colonna `Address`, `City`e `Country` usando il codice seguente:

[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Questo codice assegna in modo ingenuo i valori di indirizzo, città e paese passati al `SuppliersRow` nel `SuppliersDataTable` indipendentemente dal fatto che i valori siano stati modificati o meno. Queste modifiche determinano che la proprietà `RowState` `SuppliersRow` s venga contrassegnata come modificata. Quando viene chiamato il metodo di accesso ai dati `Update`, il `SupplierRow` è stato modificato e pertanto invia un comando `UPDATE` al database.

Si supponga, tuttavia, che sia stato aggiunto codice a questo metodo per assegnare solo i valori Address, City e Country passati se si differenziano dai valori esistenti di `SuppliersRow`. Nel caso in cui l'indirizzo, la città e il paese corrispondano ai dati esistenti, non verrà apportata alcuna modifica e il `SupplierRow` s `RowState` verrà lasciato contrassegnato come invariato. Il risultato finale è che, quando viene chiamato il metodo DAL `Update`, non viene effettuata alcuna chiamata al database perché il `SuppliersRow` non è stato modificato.

Per applicare questa modifica, sostituire le istruzioni che assegnano in modo cieco i valori Address, City e Country passati con il codice seguente:

[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

Con questo codice aggiunto, il metodo DAL `Update` Invia un'istruzione `UPDATE` al database solo per i record i cui valori correlati all'indirizzo sono stati modificati.

In alternativa, è possibile tenere traccia della presenza di differenze tra i campi degli indirizzi passati e i dati del database e, se non sono presenti, ignorare semplicemente la chiamata al metodo DAL `Update`. Questo approccio funziona bene se si usa il metodo diretto DB, perché il metodo diretto del database non ha superato un'istanza di `SuppliersRow` di cui è possibile verificare la `RowState` per determinare se una chiamata al database è effettivamente necessaria.

> [!NOTE]
> Ogni volta che viene richiamato il metodo `UpdateSupplierAddress`, viene eseguita una chiamata al database per recuperare le informazioni sul record aggiornato. Quindi, in caso di modifiche ai dati, viene eseguita un'altra chiamata al database per aggiornare la riga della tabella. Questo flusso di lavoro può essere ottimizzato creando un overload del metodo `UpdateSupplierAddress` che accetta un'istanza di `EmployeesDataTable` con *tutte* le modifiche apportate dalla pagina `BatchUpdate.aspx`. Quindi, potrebbe effettuare una chiamata al database per ottenere tutti i record dalla tabella `Suppliers`. I due set di risultati possono quindi essere enumerati e solo i record in cui si sono verificate le modifiche potrebbero essere aggiornati.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come creare un oggetto DataList completamente modificabile, consentendo a un utente di modificare rapidamente le informazioni sugli indirizzi per più fornitori. È stata avviata definendo l'interfaccia di modifica di un controllo Web TextBox per i valori Address, City e Country del fornitore nel `ItemTemplate`di DataList. Successivamente, sono stati aggiunti i pulsanti Aggiorna tutti sopra e sotto il DataList. Dopo che un utente ha apportato le modifiche e selezionato uno dei pulsanti Aggiorna tutti, vengono enumerati i `DataListItem` s e viene eseguita una chiamata al metodo della classe `SuppliersBLL` s `UpdateSupplierAddress`.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Zack Jones e Ken Pespisa. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [Successivo](handling-bll-and-dal-level-exceptions-vb.md)
