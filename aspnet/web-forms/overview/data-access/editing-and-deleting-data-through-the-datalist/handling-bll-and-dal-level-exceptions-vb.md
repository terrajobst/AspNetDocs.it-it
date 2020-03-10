---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: Gestione delle eccezioni a livello BLL e DAL (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrato come gestire in modo discreto le eccezioni generate durante un flusso di lavoro di aggiornamento di DataList modificabile.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 319ab44f2e65afc77f6f89ca8aa58c529f40d05c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78593667"
---
# <a name="handling-bll--and-dal-level-exceptions-vb"></a>Gestione delle eccezioni a livello BLL e DAL (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) o [scaricare il file PDF](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> In questa esercitazione verrà illustrato come gestire in modo discreto le eccezioni generate durante un flusso di lavoro di aggiornamento di DataList modificabile.

## <a name="introduction"></a>Introduzione

Nella [Panoramica della modifica e dell'eliminazione dei dati nell'esercitazione di DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) è stato creato un DataList che offre funzionalità semplici di modifica ed eliminazione. Sebbene sia completamente funzionante, non era facile da usare, poiché tutti gli errori che si sono verificati durante il processo di modifica o eliminazione causavano un'eccezione non gestita. Se, ad esempio, si omette il nome del prodotto o quando si modifica un prodotto, l'immissione di un valore di prezzo molto conveniente!, genera un'eccezione. Poiché questa eccezione non viene rilevata nel codice, si propaga al runtime ASP.NET, che Visualizza i dettagli dell'eccezione nella pagina Web.

Come si è visto nella [gestione delle eccezioni di livello BLL e dal in un'esercitazione della pagina ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) , se viene generata un'eccezione dalle profondità della logica di business o dei livelli di accesso ai dati, i dettagli dell'eccezione vengono restituiti a ObjectDataSource e quindi a GridView. È stato illustrato come gestire correttamente queste eccezioni creando `Updated` o `RowUpdated` gestori eventi per ObjectDataSource o GridView, verificando la presenza di un'eccezione e quindi indicando che l'eccezione è stata gestita.

Le esercitazioni di DataList, tuttavia, non utilizzano ObjectDataSource per l'aggiornamento e l'eliminazione dei dati. Si sta invece lavorando direttamente con il servizio BLL. Per rilevare le eccezioni originate da BLL o DAL, è necessario implementare il codice di gestione delle eccezioni all'interno del code-behind della pagina ASP.NET. In questa esercitazione verrà illustrato come gestire con maggiore cautela le eccezioni generate durante un flusso di lavoro di aggiornamento modificabile di DataList.

> [!NOTE]
> In *una panoramica della modifica e dell'eliminazione dei dati nell'esercitazione su DataList* sono state illustrate diverse tecniche per la modifica e l'eliminazione dei dati dal DataList, alcune tecniche relative all'utilizzo di ObjectDataSource per l'aggiornamento e l'eliminazione. Se si utilizzano queste tecniche, è possibile gestire le eccezioni da BLL o DAL tramite il `Updated` di ObjectDataSource o i gestori eventi di `Deleted`.

## <a name="step-1-creating-an-editable-datalist"></a>Passaggio 1: creazione di un oggetto DataList modificabile

Prima di preoccuparsi di gestire le eccezioni che si verificano durante il flusso di lavoro di aggiornamento, è necessario creare un DataList modificabile. Aprire la pagina `ErrorHandling.aspx` nella cartella `EditDeleteDataList`, aggiungere un DataList alla finestra di progettazione, impostare la relativa proprietà `ID` su `Products`e aggiungere un nuovo ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per l'uso del metodo di `GetProducts()` della classe `ProductsBLL` per la selezione di record; impostare gli elenchi a discesa nelle schede Inserisci, aggiorna ed Elimina su (nessuno).

[![restituire le informazioni sul prodotto utilizzando il metodo GetProducts ()](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Figura 1**: restituire le informazioni sul prodotto utilizzando il metodo `GetProducts()` ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))

Dopo aver completato la procedura guidata ObjectDataSource, Visual Studio creerà automaticamente una `ItemTemplate` per DataList. Sostituire con un `ItemTemplate` che Visualizza il nome e il prezzo di ogni prodotto e include un pulsante di modifica. Successivamente, creare un `EditItemTemplate` con un controllo Web TextBox per i pulsanti nome, prezzo e aggiornamento e Annulla. Infine, impostare la proprietà `RepeatColumns` di DataList su 2.

Dopo queste modifiche, il markup dichiarativo della pagina avrà un aspetto simile al seguente. Verificare che i pulsanti modifica, Annulla e aggiorna includano le proprietà `CommandName` impostate rispettivamente su modifica, Annulla e aggiorna.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> Per questa esercitazione è necessario abilitare lo stato di visualizzazione di DataList.

Esaminare lo stato di avanzamento tramite un browser (vedere la figura 2).

[![ogni prodotto include un pulsante di modifica](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Figura 2**: ogni prodotto include un pulsante modifica ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))

Attualmente, il pulsante modifica causa solo un postback che non rende ancora modificabile il prodotto. Per abilitare la modifica, è necessario creare gestori eventi per gli eventi `EditCommand`, `CancelCommand`e `UpdateCommand` di DataList. Gli eventi `EditCommand` e `CancelCommand` aggiornano semplicemente la proprietà `EditItemIndex` di DataList e riassociano i dati a DataList:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

Il gestore eventi `UpdateCommand` è un po' più impegnativo. È necessario leggere il prodotto modificato `ProductID` dalla raccolta `DataKeys` insieme al nome e al prezzo del prodotto dalle caselle di testo del `EditItemTemplate`, quindi chiamare il metodo `UpdateProduct` della classe `ProductsBLL` prima di restituire DataList allo stato di pre-modifica.

Per il momento, è sufficiente utilizzare esattamente lo stesso codice del gestore dell'evento `UpdateCommand` nella *Panoramica della modifica e dell'eliminazione dei dati nell'esercitazione su DataList* . Il codice verrà aggiunto per gestire normalmente le eccezioni nel passaggio 2.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

In presenza di un input non valido che può essere sotto forma di prezzo unitario formattato in modo errato, un valore del prezzo unitario non valido, ad esempio-$5,00, o l'omissione del nome del prodotto, viene generata un'eccezione. Poiché il gestore dell'evento `UpdateCommand` non include il codice di gestione delle eccezioni a questo punto, l'eccezione viene propagata al runtime di ASP.NET, dove verrà visualizzata all'utente finale (vedere la figura 3).

![Quando si verifica un'eccezione non gestita, l'utente finale Visualizza una pagina di errore](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Figura 3**: quando si verifica un'eccezione non gestita, l'utente finale Visualizza una pagina di errore

## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Passaggio 2: gestione normale delle eccezioni nel gestore eventi UpdateCommand

Durante il flusso di lavoro di aggiornamento, le eccezioni possono verificarsi nel gestore dell'evento `UpdateCommand`, nel livello BLL o dal. Se, ad esempio, un utente immette un prezzo troppo costoso, l'istruzione `Decimal.Parse` nel gestore dell'evento `UpdateCommand` genererà un'eccezione `FormatException`. Se l'utente omette il nome del prodotto o se il prezzo ha un valore negativo, il DAL genererà un'eccezione.

Quando si verifica un'eccezione, si desidera visualizzare un messaggio informativo all'interno della pagina stessa. Aggiungere un controllo Web Label alla pagina il cui `ID` è impostato su `ExceptionDetails`. Configurare il testo dell'etichetta da visualizzare in un tipo di carattere rosso, molto grande, grassetto e corsivo assegnando la relativa proprietà `CssClass` alla classe `Warning` CSS, definita nel file `Styles.css`.

Quando si verifica un errore, si vuole che l'etichetta venga visualizzata una sola volta. Ovvero nei postback successivi, il messaggio di avviso dell'etichetta dovrebbe scomparire. Questa operazione può essere eseguita cancellando la proprietà `Text` dell'etichetta o impostando la relativa proprietà `Visible` su `False` nel gestore eventi `Page_Load` (come è stato fatto di nuovo nella [gestione delle eccezioni a livello di BLL e dal in un'esercitazione della pagina ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) ) o disabilitando il supporto dello stato di visualizzazione dell'etichetta. Consente di utilizzare la seconda opzione.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Quando viene generata un'eccezione, i dettagli dell'eccezione verranno assegnati alla proprietà `Text` del controllo Label `ExceptionDetails`. Poiché lo stato di visualizzazione è disabilitato, nei postback successivi le modifiche programmatiche del `Text` proprietà andranno perse, ripristinando il testo predefinito (una stringa vuota), nascondendo quindi il messaggio di avviso.

Per determinare quando è stato generato un errore per visualizzare un messaggio utile nella pagina, è necessario aggiungere un blocco di `Try ... Catch` al gestore dell'evento `UpdateCommand`. La parte `Try` contiene codice che può causare un'eccezione, mentre il blocco `Catch` contiene codice eseguito in un'eccezione. Per ulteriori informazioni sul blocco `Try ... Catch`, vedere la sezione [nozioni fondamentali sulla gestione delle eccezioni](https://msdn.microsoft.com/library/2w8f0bss.aspx) nella documentazione di .NET Framework.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Quando viene generata un'eccezione di qualsiasi tipo dal codice all'interno del blocco `Try`, viene avviata l'esecuzione del codice del blocco di `Catch`. Il tipo di eccezione generata `DbException`, `NoNullAllowedException`, `ArgumentException`e così via dipende da cosa, esattamente, ha provocato l'errore nella prima posizione. Se si verifica un problema a livello di database, verrà generata un'`DbException`. Se viene immesso un valore non valido per i campi `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`o `ReorderLevel`, verrà generata un'`ArgumentException`, in quanto è stato aggiunto il codice per convalidare questi valori di campo nella classe `ProductsDataTable` (vedere l'esercitazione [creazione di un livello di logica di business](../introduction/creating-a-business-logic-layer-vb.md) ).

È possibile fornire una spiegazione più utile all'utente finale basando il testo del messaggio sul tipo di eccezione intercettata. Il codice seguente, che è stato usato in un formato quasi identico nell'esercitazione relativa alla [gestione delle eccezioni a livello BLL e dal in una pagina ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) , fornisce questo livello di dettaglio:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Per completare questa esercitazione, è sufficiente chiamare il metodo `DisplayExceptionDetails` dal blocco `Catch` passato nell'istanza di `Exception` intercettata (`ex`).

Con il blocco `Try ... Catch` sul posto, agli utenti viene visualizzato un messaggio di errore più informativo, come illustrato nelle figure 4 e 5. Si noti che in presenza di un'eccezione, DataList rimane in modalità di modifica. Questo perché, una volta che si è verificata l'eccezione, il flusso di controllo viene immediatamente reindirizzato al blocco `Catch`, ignorando il codice che restituisce DataList allo stato di pre-modifica.

[![viene visualizzato un messaggio di errore se un utente omette un campo obbligatorio](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Figura 4**: viene visualizzato un messaggio di errore se un utente omette un campo obbligatorio ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))

[![viene visualizzato un messaggio di errore quando si immette un prezzo negativo](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Figura 5**: viene visualizzato un messaggio di errore quando si immette un prezzo negativo ([fare clic per visualizzare l'immagine con dimensioni complete](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))

## <a name="summary"></a>Riepilogo

GridView e ObjectDataSource forniscono gestori di eventi di post-livello che includono informazioni su tutte le eccezioni generate durante il flusso di lavoro di aggiornamento ed eliminazione, nonché proprietà che possono essere impostate per indicare se l'eccezione è stata o meno. gestito. Queste funzionalità, tuttavia, non sono disponibili quando si utilizza DataList e si utilizza direttamente il servizio BLL. Al contrario, l'implementazione della gestione delle eccezioni è responsabile.

In questa esercitazione è stato illustrato come aggiungere la gestione delle eccezioni a un flusso di lavoro di aggiornamento di DataList modificabile aggiungendo un blocco di `Try ... Catch` al gestore eventi di `UpdateCommand`. Se viene generata un'eccezione durante il flusso di lavoro di aggiornamento, viene eseguito il codice `Catch` blocco s, visualizzando informazioni utili nell'etichetta `ExceptionDetails`.

A questo punto, DataList non si impegna a impedire che si verifichino eccezioni in primo luogo. Anche se si è certi che un prezzo negativo provocherà un'eccezione, non sono ancora state aggiunte funzionalità per impedire in modo proattivo a un utente di immettere tale input non valido. Nell'esercitazione successiva verrà illustrato come ridurre le eccezioni causate dall'input dell'utente non valido aggiungendo i controlli di convalida nel `EditItemTemplate`.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Linee guida di progettazione delle eccezioni](https://msdn.microsoft.com/library/ms298399.aspx)
- [Errore di registrazione di moduli e gestori (ELMAH)](http://workspaces.gotdotnet.com/elmah) (libreria open source per la registrazione degli errori)
- [Enterprise Library per .NET Framework 2,0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (include il blocco applicazione di gestione delle eccezioni)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Ken Pespisa. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](performing-batch-updates-vb.md)
> [Successivo](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
