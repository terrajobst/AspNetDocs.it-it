---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: Configurazione della connessione del livello di accesso ai dati e delle impostazioni a livelloC#di comando () | Microsoft Docs
author: rick-anderson
description: Gli oggetti TableAdapter in un set di dati tipizzato eseguono automaticamente la connessione al database, l'invio di comandi e il popolamento di un oggetto DataTable con i risultati...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: 8fe7a5a2e410b47c8c2be62851f2b7b775d60209
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74604896"
---
# <a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>Configurazione delle impostazioni del livello di accesso ai dati a livello di connessione e di comando (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip) o [Scarica PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> Gli oggetti TableAdapter in un set di dati tipizzato eseguono automaticamente la connessione al database, l'esecuzione di comandi e il popolamento di una DataTable con i risultati. In alcuni casi, tuttavia, si desidera occuparsi di questi dettagli e in questa esercitazione viene illustrato come accedere alle impostazioni a livello di comando e di connessione al database nell'oggetto TableAdapter.

## <a name="introduction"></a>Introduzione

In tutta la serie di esercitazioni sono stati usati DataSet tipizzati per implementare il livello di accesso ai dati e gli oggetti business dell'architettura a più livelli. Come illustrato nella [prima esercitazione](../introduction/creating-a-data-access-layer-cs.md), le tabelle dati del set di dati tipizzato fungono da repository di dati, mentre gli oggetti TableAdapter fungono da wrapper per comunicare con il database per recuperare e modificare i dati sottostanti. Gli oggetti TableAdapter incapsulano la complessità insita nell'utilizzo del database e non devono scrivere codice per connettersi al database, eseguire un comando o popolare i risultati in un oggetto DataTable.

In alcuni casi, tuttavia, quando è necessario inserire le profondità del TableAdapter e scrivere codice che interagisce direttamente con gli oggetti ADO.NET. Nell'esercitazione relativa al [wrapping delle modifiche al database all'interno di una transazione](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) , ad esempio, sono stati aggiunti metodi a TableAdapter per iniziare, eseguire il commit e il rollback delle transazioni di ADO.NET. Questi metodi usavano un oggetto `SqlTransaction` interno creato manualmente e assegnato agli oggetti `SqlCommand` di TableAdapter.

In questa esercitazione verrà illustrato come accedere alla connessione di database e alle impostazioni a livello di comando nell'oggetto TableAdapter. In particolare, si aggiungeranno funzionalità al `ProductsTableAdapter` che consente l'accesso alla stringa di connessione sottostante e alle impostazioni di timeout dei comandi.

## <a name="working-with-data-using-adonet"></a>Uso dei dati con ADO.NET

Il Framework Microsoft .NET contiene una pletora di classi progettate specificamente per lavorare con i dati. Queste classi, disponibili nello [spazio dei nomi`System.Data`](https://msdn.microsoft.com/library/system.data.aspx), sono definite classi *ADO.NET* . Alcune delle classi in ADO.NET Umbrella sono associate a un *provider di dati*specifico. È possibile considerare un provider di dati come canale di comunicazione che consente il flusso di informazioni tra le classi ADO.NET e l'archivio dati sottostante. Sono disponibili provider generalizzati, ad esempio OleDb e ODBC, nonché provider progettati appositamente per un particolare sistema di database. Se, ad esempio, è possibile connettersi a un database di Microsoft SQL Server utilizzando il provider OleDb, il provider SqlClient è molto più efficiente in quanto è stato progettato e ottimizzato in modo specifico per SQL Server.

Quando si accede a livello di codice ai dati, viene comunemente usato il modello seguente:

- Stabilire una connessione al database.
- Eseguire un comando.
- Per `SELECT` query, usare i record risultanti.

Sono disponibili classi ADO.NET separate per eseguire ognuno di questi passaggi. Per connettersi a un database utilizzando il provider SqlClient, ad esempio, utilizzare la [classe`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Per eseguire un comando `INSERT`, `UPDATE`, `DELETE`o `SELECT` per il database, usare la [classe`SqlCommand`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Fatta eccezione per le [modifiche al database di wrapping in un'](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) esercitazione sulle transazioni, non è stato necessario scrivere codice di ADO.NET di basso livello perché il codice generato automaticamente da TableAdapter include la funzionalità necessaria per connettersi al database, eseguire comandi, recuperare dati e popolare tali dati in DataTable. Tuttavia, in alcuni casi è necessario personalizzare queste impostazioni di basso livello. Nei prossimi passaggi verrà illustrato come attingere agli oggetti ADO.NET usati internamente dagli oggetti TableAdapter.

## <a name="step-1-examining-with-the-connection-property"></a>Passaggio 1: analisi con la proprietà di connessione

Ogni classe TableAdapter dispone di una proprietà `Connection` che specifica le informazioni di connessione al database. Il tipo di dati e il valore `ConnectionString` della proprietà sono determinati dalle selezioni effettuate nella configurazione guidata TableAdapter. Quando si aggiunge un TableAdapter a un DataSet tipizzato, questa procedura guidata richiede l'origine del database (vedere la figura 1). Nell'elenco a discesa del primo passaggio sono inclusi i database specificati nel file di configurazione, nonché tutti gli altri database nelle connessioni dati di Esplora server s. Se il database che si desidera utilizzare non è presente nell'elenco a discesa, è possibile specificare una nuova connessione al database facendo clic sul pulsante nuova connessione e fornendo le informazioni di connessione necessarie.

[![il primo passaggio della configurazione guidata TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**Figura 1**: primo passaggio della configurazione guidata TableAdapter ([fare clic per visualizzare l'immagine con dimensioni complete](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))

È necessario esaminare il codice per la proprietà `Connection` TableAdapter. Come indicato nell'esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) , è possibile visualizzare il codice TableAdapter generato automaticamente andando alla finestra Visualizzazione classi, eseguendo il drill-down fino alla classe appropriata, quindi facendo doppio clic sul nome del membro.

Passare alla finestra Visualizzazione classi dal menu Visualizza e scegliere Visualizzazione classi (oppure premere CTRL + MAIUSC + C). Dalla metà superiore della finestra Visualizzazione classi eseguire il drill-down allo spazio dei nomi `NorthwindTableAdapters` e selezionare la classe `ProductsTableAdapter`. In questo modo vengono visualizzati i membri `ProductsTableAdapter` nella metà inferiore del Visualizzazione classi, come illustrato nella figura 2. Fare doppio clic sulla proprietà `Connection` per visualizzarne il codice.

![Fare doppio clic sulla proprietà di connessione nel Visualizzazione classi per visualizzare il codice generato automaticamente](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**Figura 2**: fare doppio clic sulla proprietà di connessione nel Visualizzazione classi per visualizzare il codice generato automaticamente

La proprietà `Connection` degli oggetti TableAdapter e altro codice correlato alla connessione è la seguente:

[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

Quando viene creata un'istanza della classe TableAdapter, la variabile membro `_connection` è uguale a `null`. Quando si accede alla proprietà `Connection`, verifica innanzitutto se è stata creata un'istanza della variabile membro `_connection`. In caso contrario, viene richiamato il metodo `InitConnection`, che crea un'istanza di `_connection` e imposta la relativa proprietà `ConnectionString` sul valore della stringa di connessione specificato dal primo passaggio della configurazione guidata TableAdapter.

È inoltre possibile assegnare la proprietà `Connection` a un oggetto `SqlConnection`. In questo modo il nuovo oggetto `SqlConnection` viene associato a ognuno degli oggetti `SqlCommand` di TableAdapter.

## <a name="step-2-exposing-connection-level-settings"></a>Passaggio 2: esposizione delle impostazioni a livello di connessione

Le informazioni di connessione devono rimanere incapsulate all'interno del TableAdapter e non essere accessibili ad altri livelli nell'architettura dell'applicazione. Potrebbero tuttavia verificarsi scenari in cui le informazioni a livello di connessione di TableAdapter devono essere accessibili o personalizzabili per una pagina di query, utente o ASP.NET.

Consente di estendere il `ProductsTableAdapter` nel set di dati `Northwind` per includere una proprietà `ConnectionString` che può essere utilizzata dal livello della logica di business per leggere o modificare la stringa di connessione utilizzata dal TableAdapter.

> [!NOTE]
> Una *stringa di connessione* è una stringa che specifica le informazioni di connessione al database, ad esempio il provider da utilizzare, il percorso del database, le credenziali di autenticazione e altre impostazioni relative al database. Per un elenco di modelli di stringa di connessione usati da un'ampia gamma di archivi dati e provider, vedere [connectionStrings.com](http://www.connectionstrings.com/).

Come illustrato nell'esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) , è possibile estendere le classi generate automaticamente del set di dati tipizzato tramite l'utilizzo di classi parziali. Per prima cosa, creare una nuova sottocartella nel progetto denominato `ConnectionAndCommandSettings` sotto la cartella `~/App_Code/DAL`.

![Aggiungere una sottocartella denominata ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**Figura 3**: aggiungere una sottocartella denominata `ConnectionAndCommandSettings`

Aggiungere un nuovo file di classe denominato `ProductsTableAdapter.ConnectionAndCommandSettings.cs` e immettere il codice seguente:

[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

Questa classe parziale aggiunge una proprietà di `public` denominata `ConnectionString` alla classe `ProductsTableAdapter` che consente a qualsiasi livello di leggere o aggiornare la stringa di connessione per la connessione sottostante di TableAdapter.

Con questa classe parziale creata (e salvata), aprire la classe `ProductsBLL`. Passare a uno dei metodi esistenti e digitare `Adapter` e quindi premere il tasto periodo per visualizzare IntelliSense. Verrà visualizzata la nuova proprietà `ConnectionString` disponibile in IntelliSense, ovvero è possibile leggere o modificare a livello di codice questo valore da BLL.

## <a name="exposing-the-entire-connection-object"></a>Esposizione dell'intero oggetto Connection

Questa classe parziale espone una sola proprietà dell'oggetto connessione sottostante: `ConnectionString`. Se si desidera rendere disponibile l'intero oggetto Connection oltre i confini del TableAdapter, è possibile modificare in alternativa il livello di protezione della proprietà `Connection`. Il codice generato automaticamente esaminato nel passaggio 1 ha mostrato che la proprietà `Connection` di TableAdapter è contrassegnata come `internal`, ovvero è possibile accedervi solo tramite le classi nello stesso assembly. Questo può essere modificato, tuttavia, tramite la proprietà `ConnectionModifier` di TableAdapter.

Aprire il set di dati `Northwind`, fare clic sul `ProductsTableAdapter` nella finestra di progettazione e passare al Finestra Proprietà. Qui viene visualizzato il `ConnectionModifier` impostato sul valore predefinito, `Assembly`. Per rendere disponibile la proprietà `Connection` al di fuori dell'assembly DataSet tipizzato, modificare la proprietà `ConnectionModifier` in `Public`.

[![è possibile configurare il livello di accessibilità della proprietà di connessione tramite la proprietà ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**Figura 4**: è possibile configurare il livello di accessibilità della proprietà `Connection` tramite la proprietà `ConnectionModifier` ([fare clic per visualizzare l'immagine con dimensioni complete](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))

Salvare il set di dati e quindi tornare alla classe `ProductsBLL`. Come in precedenza, passare a uno dei metodi esistenti e digitare `Adapter` e quindi premere il tasto periodo per visualizzare IntelliSense. L'elenco deve includere una proprietà `Connection`, ovvero è ora possibile leggere o assegnare a livello di codice le impostazioni a livello di connessione da BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Passaggio 3: analisi delle proprietà correlate ai comandi

Un TableAdapter è costituito da una query principale che, per impostazione predefinita, include istruzioni `INSERT`, `UPDATE`e `DELETE` generate automaticamente. Le istruzioni `INSERT`, `UPDATE`e `DELETE` della query principale sono implementate nel codice di TableAdapter come oggetto adattatore dati ADO.NET tramite la proprietà `Adapter`. Analogamente alla proprietà `Connection`, il tipo di dati della proprietà `Adapter` è determinato dal provider di dati utilizzato. Poiché in queste esercitazioni viene utilizzato il provider SqlClient, la proprietà `Adapter` è di tipo [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

La proprietà `Adapter` di TableAdapter presenta tre proprietà di tipo `SqlCommand` che utilizza per emettere le istruzioni `INSERT`, `UPDATE`e `DELETE`:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Un oggetto `SqlCommand` è responsabile dell'invio di una particolare query al database e include proprietà quali: [`CommandText`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), che contiene l'istruzione SQL ad hoc o stored procedure da eseguire; e [`Parameters`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), ovvero una raccolta di oggetti `SqlParameter`. Come è stato illustrato nell'esercitazione [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) , questi oggetti comando possono essere personalizzati tramite il finestra Proprietà.

Oltre alla query principale, il TableAdapter può includere un numero variabile di metodi che, quando vengono richiamati, inviano un comando specificato al database. L'oggetto comando della query principale e gli oggetti Command per tutti i metodi aggiuntivi vengono archiviati nella proprietà `CommandCollection` di TableAdapter.

Per esaminare il codice generato dal `ProductsTableAdapter` nel set di dati `Northwind` per queste due proprietà e le variabili membro di supporto e i metodi helper:

[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

Il codice per le proprietà `Adapter` e `CommandCollection` è strettamente simulato della proprietà `Connection`. Sono presenti variabili membro che contengono gli oggetti utilizzati dalle proprietà. Le proprietà `get` funzioni di accesso iniziano verificando se la variabile membro corrispondente è `null`. In tal caso, viene chiamato un metodo di inizializzazione che crea un'istanza della variabile membro e assegna le proprietà correlate al comando di base.

## <a name="step-4-exposing-command-level-settings"></a>Passaggio 4: esposizione delle impostazioni a livello di comando

Idealmente, le informazioni a livello di comando devono rimanere incapsulate all'interno del livello di accesso ai dati. Se queste informazioni sono necessarie in altri livelli dell'architettura, tuttavia, possono essere esposte tramite una classe parziale, proprio come con le impostazioni a livello di connessione.

Poiché il TableAdapter dispone solo di una singola proprietà `Connection`, il codice per esporre le impostazioni a livello di connessione è piuttosto semplice. Quando si modificano le impostazioni a livello di comando, il TableAdapter può avere più oggetti Command, ovvero un `InsertCommand`, `UpdateCommand`e `DeleteCommand`, insieme a un numero variabile di oggetti Command nella proprietà `CommandCollection`. Quando si aggiornano le impostazioni a livello di comando, queste impostazioni devono essere propagate a tutti gli oggetti Command.

Si supponga, ad esempio, che nel TableAdapter siano presenti alcune query che hanno richiesto un tempo molto lungo per l'esecuzione. Quando si usa il TableAdapter per eseguire una di queste query, potrebbe essere necessario aumentare la [proprietà`CommandTimeout`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx)oggetto comando. Questa proprietà specifica il numero di secondi di attesa per l'esecuzione del comando e il valore predefinito è 30.

Per consentire la regolazione della proprietà `CommandTimeout` da parte di BLL, aggiungere il seguente metodo `public` al `ProductsDataTable` utilizzando il file di classe parziale creato nel passaggio 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`):

[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

Questo metodo può essere richiamato dal livello BLL o Presentation per impostare il timeout del comando per tutti i comandi che vengono risolti da tale istanza di TableAdapter.

> [!NOTE]
> Le proprietà `Adapter` e `CommandCollection` sono contrassegnate come `private`, ovvero è possibile accedervi solo dal codice all'interno del TableAdapter. A differenza della proprietà `Connection`, questi modificatori di accesso non sono configurabili. Se pertanto è necessario esporre proprietà a livello di comando ad altri livelli nell'architettura, è necessario utilizzare l'approccio della classe parziale illustrato in precedenza per fornire un metodo `public` o una proprietà che legge o scrive negli oggetti comando `private`.

## <a name="summary"></a>Riepilogo

Gli oggetti TableAdapter in un DataSet tipizzato servono a incapsulare i dettagli di accesso ai dati e la complessità. Utilizzando gli oggetti TableAdapter, non è necessario preoccuparsi di scrivere codice ADO.NET per connettersi al database, eseguire un comando o popolare i risultati in un oggetto DataTable. Viene gestito automaticamente.

Tuttavia, in alcuni casi è necessario personalizzare le specifiche di ADO.NET di basso livello, ad esempio la modifica della stringa di connessione o la connessione predefinita o i valori di timeout dei comandi. Per impostazione predefinita, il TableAdapter dispone di proprietà `Connection`, `Adapter`e `CommandCollection` generate automaticamente. tali proprietà sono `internal` o `private`. Queste informazioni interne possono essere esposte estendendo il TableAdapter usando le classi parziali per includere `public` metodi o proprietà. In alternativa, è possibile configurare il modificatore di accesso alla proprietà `Connection` di TableAdapter tramite la proprietà `ConnectionModifier` di TableAdapter.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Burnadette Leigh, S Ren Jacob Lauritsen, Teresa Murphy e Hilton Geisenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](working-with-computed-columns-cs.md)
> [Successivo](protecting-connection-strings-and-other-configuration-information-cs.md)
