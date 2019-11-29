---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: Debug di stored procedure (VB) | Microsoft Docs
author: rick-anderson
description: Le edizioni Visual Studio Professional e Team System consentono di impostare punti di interruzione ed eseguire un'istruzione alle stored procedure all'interno di SQL Server, rendendo archiviato il debug...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: 13d213ef4baf493a4f05a82daae8d2dc3b0aa61b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74604558"
---
# <a name="debugging-stored-procedures-vb"></a>Debug di stored procedure (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip) o [Scarica PDF](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> Le edizioni Visual Studio Professional e Team System consentono di impostare punti di interruzione ed eseguire un'istruzione nelle stored procedure all'interno di SQL Server, semplificando il debug delle stored procedure per il debug del codice dell'applicazione. Questa esercitazione illustra il debug diretto del database e il debug di applicazioni di stored procedure.

## <a name="introduction"></a>Introduzione

Visual Studio offre un'esperienza di debug avanzata. Con poche sequenze di tasti o clic del mouse, è possibile usare i punti di interruzione per arrestare l'esecuzione di un programma ed esaminarne lo stato e il flusso di controllo. Oltre al debug del codice dell'applicazione, Visual Studio offre il supporto per il debug di stored procedure dall'interno SQL Server. Analogamente ai punti di interruzione possono essere impostati all'interno del codice di una classe code-behind ASP.NET o di una classe del livello della logica di business, anche se possono essere inseriti all'interno di stored procedure.

In questa esercitazione verrà illustrato come eseguire le stored procedure dall'Esplora server all'interno di Visual Studio e come impostare i punti di interruzione che vengono raggiunti quando il stored procedure viene chiamato dall'applicazione ASP.NET in esecuzione.

> [!NOTE]
> Sfortunatamente, le stored procedure possono essere sottoposte a debug solo tramite le versioni Professional e team Systems di Visual Studio. Se si usa Visual Web Developer o la versione standard di Visual Studio, è possibile leggere insieme ai passaggi necessari per eseguire il debug delle stored procedure, ma non sarà possibile replicare questi passaggi nel computer.

## <a name="sql-server-debugging-concepts"></a>Concetti relativi al debug di SQL Server

Microsoft SQL Server 2005 è stato progettato per fornire l'integrazione con [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), ovvero il runtime utilizzato da tutti gli assembly .NET. Di conseguenza, SQL Server 2005 supporta oggetti di database gestiti. Ovvero, è possibile creare oggetti di database come stored procedure e funzioni definite dall'utente come metodi in una classe di Visual Basic. In questo modo, le stored procedure e le funzioni definite dall'utente utilizzeranno le funzionalità nell'.NET Framework e dalle classi personalizzate. Naturalmente, SQL Server 2005 fornisce anche il supporto per gli oggetti di database T-SQL.

SQL Server 2005 offre supporto per il debug sia per gli oggetti T-SQL che per gli oggetti di database gestiti. Tuttavia, è possibile eseguire il debug di questi oggetti solo tramite le edizioni di Visual Studio 2005 Professional e team Systems. In questa esercitazione verrà esaminato il debug di oggetti di database T-SQL. L'esercitazione successiva esamina il debug di oggetti di database gestiti.

La [Panoramica del debug di T-SQL e CLR nel SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) di Blog del [team di integrazione clr di SQL Server 2005](https://blogs.msdn.com/sqlclr/default.aspx) evidenzia i tre modi per eseguire il debug di SQL Server 2005 oggetti da Visual Studio:

- **Debug di database diretti (DDD)** : da Esplora server è possibile eseguire istruzioni su qualsiasi oggetto di database T-SQL, ad esempio stored procedure e funzioni definite dall'utente. Nel passaggio 1 viene esaminato DDD.
- **Debug dell'applicazione** : è possibile impostare punti di interruzione in un oggetto di database e quindi eseguire l'applicazione ASP.NET. Quando l'oggetto di database viene eseguito, il punto di interruzione viene raggiunto e il controllo viene convertito nel debugger. Si noti che con il debug dell'applicazione non è possibile eseguire un'istruzione in un oggetto di database dal codice dell'applicazione. È necessario impostare in modo esplicito i punti di interruzione nelle stored procedure o nelle funzioni definite dall'utente in cui si desidera arrestare il debugger. Il debug dell'applicazione viene esaminato a partire dal passaggio 2.
- Il **debug da un SQL Server Project** -Visual Studio Professional e team Systems Edition includono un tipo di progetto SQL Server comunemente utilizzato per creare oggetti di database gestiti. Nell'esercitazione successiva verrà esaminato l'utilizzo di progetti SQL Server e il relativo contenuto.

Visual Studio è in grado di eseguire il debug di stored procedure in istanze SQL Server locali e remote. Un'istanza di SQL Server locale è installata nello stesso computer di Visual Studio. Se il database di SQL Server in uso non si trova nel computer di sviluppo, viene considerato un'istanza remota. Per queste esercitazioni sono state usate istanze di SQL Server locali. Per eseguire il debug di stored procedure in un'istanza remota di SQL Server sono necessari più passaggi di configurazione rispetto al debug di stored procedure in un'istanza locale.

Se si usa un'istanza di SQL Server locale, è possibile iniziare con il passaggio 1 e completare questa esercitazione fino alla fine. Se si utilizza un'istanza di SQL Server remota, tuttavia, prima di tutto è necessario assicurarsi che, durante il debug, il computer di sviluppo venga registrato con un account utente di Windows che disponga di un account di accesso SQL Server nell'istanza remota. Inoltre, l'account di accesso al database e l'account di accesso al database utilizzati per connettersi al database dall'applicazione ASP.NET in esecuzione devono essere membri del ruolo `sysadmin`. Per ulteriori informazioni sulla configurazione di Visual Studio e SQL Server per eseguire il debug di un'istanza remota, vedere la sezione debug di oggetti di database T-SQL in istanze remote.

Infine, è importante comprendere che il supporto del debug per gli oggetti di database T-SQL non è così ricco di funzionalità come il supporto del debug per le applicazioni .NET. Ad esempio, le condizioni del punto di interruzione e i filtri non sono supportati, ma solo un subset delle finestre di debug è disponibile, non è possibile usare modifica e continuazione, il rendering della finestra immediata viene reso inutile e così via. Per ulteriori informazioni, vedere [limitazioni sui comandi e sulle funzionalità del debugger](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) .

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Passaggio 1: esecuzione diretta di una stored procedure

Visual Studio semplifica il debug diretto di un oggetto di database. Viene ora illustrato come utilizzare la funzionalità di debug di database diretto (DDD) per eseguire un'istruzione nel `Products_SelectByCategoryID` stored procedure nel database Northwind. Come suggerisce il nome, `Products_SelectByCategoryID` restituisce informazioni sul prodotto per una categoria specifica. è stato creato nell'esercitazione [utilizzo di stored procedure esistenti per il DataSet tipizzato s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) . Per iniziare, passare al Esplora server ed espandere il nodo del database Northwind. Successivamente, eseguire il drill-down nella cartella stored procedure, fare clic con il pulsante destro del mouse sul stored procedure `Products_SelectByCategoryID` e scegliere l'opzione Esegui istruzione stored procedure dal menu di scelta rapida. Verrà avviato il debugger.

Poiché il `Products_SelectByCategoryID` stored procedure prevede un parametro di input `@CategoryID`, viene richiesto di fornire questo valore. Immettere 1, che restituirà informazioni sulle bevande.

![Usare il valore 1 per il parametro @CategoryID](debugging-stored-procedures-vb/_static/image1.png)

**Figura 1**: usare il valore 1 per il parametro `@CategoryID`

Dopo aver fornito il valore per il parametro `@CategoryID`, viene eseguita la stored procedure. Anziché eseguire fino al completamento, tuttavia, il debugger interrompe l'esecuzione sulla prima istruzione. Prendere nota della freccia gialla nel margine, che indica la posizione corrente nel stored procedure. È possibile visualizzare e modificare i valori dei parametri tramite il finestra Espressioni di controllo o posizionando il puntatore del mouse sul nome del parametro nel stored procedure.

[![il debugger si è arrestato sulla prima istruzione della stored procedure](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**Figura 2**: il debugger è stato interrotto sulla prima istruzione della stored procedure ([fare clic per visualizzare l'immagine con dimensioni complete](debugging-stored-procedures-vb/_static/image4.png))

Per eseguire il stored procedure un'istruzione alla volta, fare clic sul pulsante Esegui istruzione/routine sulla barra degli strumenti o premere il tasto F10. Il `Products_SelectByCategoryID` stored procedure contiene una singola istruzione `SELECT`, quindi la funzione F10 eseguirà l'istruzione/routine della singola istruzione e completerà l'esecuzione del stored procedure. Al termine dell'stored procedure, l'output verrà visualizzato nella finestra di output e il debugger verrà terminato.

> [!NOTE]
> Il debug T-SQL viene eseguito a livello di istruzione. non è possibile eseguire istruzioni in un'istruzione `SELECT`.

## <a name="step-2-configuring-the-website-for-application-debugging"></a>Passaggio 2: configurazione del sito Web per il debug di applicazioni

Quando si esegue il debug di un stored procedure direttamente dall'Esplora server è utile, in molti scenari è più interessante eseguire il debug del stored procedure quando viene chiamato dall'applicazione ASP.NET. È possibile aggiungere punti di interruzione a un stored procedure all'interno di Visual Studio e avviare il debug dell'applicazione ASP.NET. Quando un stored procedure con punti di interruzione viene richiamato dall'applicazione, l'esecuzione si arresterà in corrispondenza del punto di interruzione e sarà possibile visualizzare e modificare i valori dei parametri della stored procedure e scorrere le istruzioni, esattamente come nel passaggio 1.

Prima di poter avviare il debug delle stored procedure chiamate dall'applicazione, è necessario indicare all'applicazione Web ASP.NET di integrarsi con il debugger SQL Server. Per iniziare, fare clic con il pulsante destro del mouse sul nome del sito Web nella Esplora soluzioni (`ASPNET_Data_Tutorial_74_VB`). Scegliere l'opzione pagine delle proprietà dal menu di scelta rapida, selezionare l'elemento Opzioni di avvio a sinistra e selezionare la casella di controllo SQL Server nella sezione debugger (vedere la figura 3).

[![selezionare la casella di controllo SQL Server nelle pagine delle proprietà dell'applicazione](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**Figura 3**: selezionare la casella di controllo SQL Server nelle pagine delle proprietà dell'applicazione ([fare clic per visualizzare l'immagine con dimensioni complete](debugging-stored-procedures-vb/_static/image7.png))

Inoltre, è necessario aggiornare la stringa di connessione del database usata dall'applicazione in modo che il pool di connessioni sia disabilitato. Quando una connessione a un database viene chiusa, l'oggetto `SqlConnection` corrispondente viene inserito in un pool di connessioni disponibili. Quando si stabilisce una connessione a un database, è possibile recuperare un oggetto connessione disponibile da questo pool anziché dover creare e stabilire una nuova connessione. Questo pool di oggetti connessione rappresenta un miglioramento delle prestazioni ed è abilitato per impostazione predefinita. Tuttavia, quando si esegue il debug si desidera disattivare il pool di connessioni perché l'infrastruttura di debug non viene ristabilita correttamente quando si utilizza una connessione ricavata dal pool.

Per disabilitare il pool di connessioni, aggiornare il `NORTHWNDConnectionString` in `Web.config` in modo che includa l'impostazione `Pooling=false`.

[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> Una volta terminato il debug SQL Server tramite l'applicazione ASP.NET, assicurarsi di ripristinare il pool di connessioni rimuovendo l'impostazione `Pooling` dalla stringa di connessione (oppure impostando il valore su `Pooling=true`).

A questo punto l'applicazione ASP.NET è stata configurata in modo da consentire a Visual Studio di eseguire il debug di SQL Server oggetti di database quando viene richiamato tramite l'applicazione Web. Non resta che aggiungere un punto di interruzione a una stored procedure e avviare il debug.

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Passaggio 3: aggiunta di un punto di interruzione e debug

Aprire il `Products_SelectByCategoryID` stored procedure e impostare un punto di interruzione all'inizio dell'istruzione `SELECT` facendo clic sul margine nella posizione appropriata o posizionando il cursore all'inizio dell'istruzione `SELECT` e premendo F9. Come illustrato nella figura 4, il punto di interruzione viene visualizzato come cerchio rosso nel margine.

[![impostare un punto di interruzione nella stored procedure Products_SelectByCategoryID](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**Figura 4**: impostare un punto di interruzione nella stored procedure `Products_SelectByCategoryID` ([fare clic per visualizzare l'immagine con dimensioni complete](debugging-stored-procedures-vb/_static/image10.png))

Per eseguire il debug di un oggetto di database SQL tramite un'applicazione client, è necessario che il database sia configurato per supportare il debug delle applicazioni. Quando si imposta un punto di interruzione per la prima volta, questa impostazione deve essere attivata automaticamente, ma è prudente eseguire un doppio controllo. Fare clic con il pulsante destro del mouse sul nodo `NORTHWND.MDF` nel Esplora server. Il menu di scelta rapida deve includere una voce di menu selezionata denominata debug applicazione.

![Verificare che l'opzione di debug dell'applicazione sia abilitata](debugging-stored-procedures-vb/_static/image11.png)

**Figura 5**: assicurarsi che l'opzione di debug dell'applicazione sia abilitata

Con il punto di interruzione impostato e l'opzione di debug dell'applicazione abilitata, è possibile eseguire il debug del stored procedure quando viene chiamato dall'applicazione ASP.NET. Avviare il debugger scegliendo Avvia debug dal menu debug, premendo F5 oppure facendo clic sull'icona di riproduzione verde sulla barra degli strumenti. Questo avvierà il debugger e avvierà il sito Web.

Il stored procedure `Products_SelectByCategoryID` è stato creato nell'esercitazione [utilizzo di stored procedure esistenti per i TableAdapter tipizzati del set di dati](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) . La pagina Web corrispondente (`~/AdvancedDAL/ExistingSprocs.aspx`) contiene un GridView che Visualizza i risultati restituiti da questo stored procedure. Visitare questa pagina tramite il browser. Al raggiungimento della pagina, il punto di interruzione nella `Products_SelectByCategoryID` stored procedure viene raggiunto e il controllo viene restituito a Visual Studio. Proprio come nel passaggio 1, è possibile eseguire le istruzioni stored procedure s e visualizzare e modificare i valori dei parametri.

[![pagina ExistingSprocs. aspx inizialmente Visualizza le bevande](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**Figura 6**: la pagina `ExistingSprocs.aspx` Visualizza inizialmente le bevande ([fare clic per visualizzare l'immagine con dimensioni complete](debugging-stored-procedures-vb/_static/image14.png))

[![è stato raggiunto il punto di interruzione della stored procedure](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**Figura 7**: è stato raggiunto il punto di interruzione della stored procedure ([fare clic per visualizzare l'immagine con dimensioni complete](debugging-stored-procedures-vb/_static/image17.png))

Come illustrato nell'finestra Espressioni di controllo della figura 7, il valore del parametro `@CategoryID` è 1. Questo è dovuto al fatto che la pagina `ExistingSprocs.aspx` Visualizza inizialmente i prodotti nella categoria bevande, il cui valore `CategoryID` è pari a 1. Scegliere una categoria diversa dall'elenco a discesa. Questa operazione provoca un postback ed esegue nuovamente il `Products_SelectByCategoryID` stored procedure. Il punto di interruzione viene raggiunto di nuovo, ma questa volta il valore del parametro `@CategoryID` riflette l'elemento dell'elenco a discesa selezionato s `CategoryID`.

[![scegliere una categoria diversa dall'elenco a discesa](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**Figura 8**: scegliere una categoria diversa dall'elenco a discesa ([fare clic per visualizzare l'immagine con dimensioni complete](debugging-stored-procedures-vb/_static/image20.png))

[![il parametro @CategoryID riflette la categoria selezionata dalla pagina Web](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**Figura 9**: il parametro `@CategoryID` riflette la categoria selezionata dalla pagina Web ([fare clic per visualizzare l'immagine con dimensioni complete](debugging-stored-procedures-vb/_static/image23.png))

> [!NOTE]
> Se il punto di interruzione nel stored procedure `Products_SelectByCategoryID` non viene raggiunto quando si visita la pagina `ExistingSprocs.aspx`, assicurarsi che la casella di controllo SQL Server sia stata selezionata nella sezione debugger della pagina delle proprietà dell'applicazione ASP.NET, che il pool di connessioni sia stato disabilitato e che l'opzione di debug dell'applicazione del database sia abilitata. Se si verificano ancora problemi, riavviare Visual Studio e riprovare.

## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Debug di oggetti di database T-SQL in istanze remote

Il debug di oggetti di database tramite Visual Studio è piuttosto semplice quando l'istanza di database SQL Server si trova nello stesso computer di Visual Studio. Tuttavia, se SQL Server e Visual Studio si trovano in computer diversi, è necessaria una configurazione attenta per ottenere tutto il funzionamento corretto. Ci sono due attività principali da affrontare:

- Verificare che l'account di accesso utilizzato per la connessione al database tramite ADO.NET appartenga al ruolo `sysadmin`.
- Verificare che l'account utente di Windows usato da Visual Studio nel computer di sviluppo sia un account di accesso SQL Server valido che appartenga al ruolo `sysadmin`.

Il primo passaggio è relativamente semplice. Per prima cosa, identificare l'account utente usato per connettersi al database dall'applicazione ASP.NET e quindi, da SQL Server Management Studio, aggiungere tale account di accesso al ruolo `sysadmin`.

La seconda attività richiede che l'account utente di Windows utilizzato per eseguire il debug dell'applicazione sia un account di accesso valido nel database remoto. Tuttavia, è probabile che l'account di Windows con cui si è effettuato l'accesso alla workstation non sia un account di accesso valido per SQL Server. Anziché aggiungere un account di accesso specifico a SQL Server, una scelta migliore consiste nel designare un account utente di Windows come account di debug SQL Server. Per eseguire il debug degli oggetti di database di un'istanza di SQL Server remota, è quindi necessario eseguire Visual Studio utilizzando le credenziali dell'account di accesso di Windows.

Un esempio dovrebbe aiutare a chiarire la situazione. Si supponga che sia presente un account di Windows denominato `SQLDebug` all'interno del dominio Windows. Questo account deve essere aggiunto all'istanza di SQL Server remota come account di accesso valido e come membro del ruolo `sysadmin`. Quindi, per eseguire il debug dell'istanza di SQL Server remota da Visual Studio, è necessario eseguire Visual Studio come utente `SQLDebug`. Questa operazione può essere eseguita tramite la disconnessione dalla workstation, l'accesso come `SQLDebug`e l'avvio di Visual Studio, ma un approccio più semplice consiste nell'accedere alla workstation usando le proprie credenziali e quindi usare `runas.exe` per avviare Visual Studio come utente `SQLDebug`. `runas.exe` consente l'esecuzione di una particolare applicazione con le sembianze di un account utente diverso. Per avviare Visual Studio come `SQLDebug`, è possibile immettere la seguente istruzione dalla riga di comando:

[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

Per una spiegazione più dettagliata di questo processo, vedere la Guida di [William R. Vaughn](http://betav.com/BLOG/billva/) s *autostop a Visual Studio e SQL Server, settima edizione* , nonché [procedura: impostare le autorizzazioni SQL Server per il debug](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Se nel computer di sviluppo è in esecuzione Windows XP Service Pack 2, sarà necessario configurare il firewall della connessione Internet per consentire il debug remoto. [L'articolo How to: Enable SQL Server 2005 Debugging](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) note che prevede due passaggi: (a) nel computer host di Visual Studio, è necessario aggiungere `Devenv.exe` all'elenco di eccezioni e aprire la porta TCP 135. e (b) nel computer remoto (SQL), è necessario aprire la porta TCP 135 e aggiungere `sqlservr.exe` all'elenco delle eccezioni. Se i criteri di dominio richiedono che le comunicazioni di rete vengano eseguite tramite IPSec, è necessario aprire le porte UDP 4500 e UDP 500.

## <a name="summary"></a>Riepilogo

Oltre a fornire supporto per il debug per il codice dell'applicazione .NET, Visual Studio offre anche un'ampia gamma di opzioni di debug per SQL Server 2005. In questa esercitazione sono state esaminate due di queste opzioni: debug diretto del database e debug delle applicazioni. Per eseguire direttamente il debug di un oggetto di database T-SQL, individuare l'oggetto tramite il Esplora server quindi fare clic con il pulsante destro del mouse su di esso e scegliere Esegui istruzione. Questa operazione avvia il debugger e si interrompe sulla prima istruzione nell'oggetto di database. in questo punto è possibile eseguire le istruzioni dell'oggetto e visualizzare e modificare i valori dei parametri. Nel passaggio 1 è stato usato questo approccio per eseguire un'istruzione nell'`Products_SelectByCategoryID` stored procedure.

Il debug dell'applicazione consente di impostare i punti di interruzione direttamente all'interno degli oggetti di database. Quando un oggetto di database con punti di interruzione viene richiamato da un'applicazione client, ad esempio un'applicazione Web ASP.NET, il programma si interrompe quando il debugger acquisisce il sopravvento. Il debug dell'applicazione è utile perché mostra in modo più chiaro quale azione dell'applicazione causa la chiamata di un determinato oggetto di database. Tuttavia, richiede un po' più di configurazione e configurazione rispetto al debug diretto del database.

È anche possibile eseguire il debug di oggetti di database tramite SQL Server progetti. Nell'esercitazione successiva verrà illustrato l'utilizzo di progetti SQL Server e di come utilizzarli per creare ed eseguire il debug di oggetti di database gestiti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](protecting-connection-strings-and-other-configuration-information-vb.md)
> [Successivo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
