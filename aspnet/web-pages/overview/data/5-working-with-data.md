---
uid: web-pages/overview/data/5-working-with-data
title: Introduzione all'uso di un database in siti Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo capitolo descrive come accedere ai dati da un database e visualizzarli usando Pagine Web ASP.NET.
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 45e988d037465e59ad352bb9444af2c69fd3cd70
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586534"
---
# <a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Introduzione all'uso di un database in siti Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come usare gli strumenti di Microsoft WebMatrix per creare un database in un sito Web di Pagine Web ASP.NET (Razor) e come creare pagine che consentono di visualizzare, aggiungere, modificare ed eliminare dati.
> 
> **Cosa si apprenderà:** 
> 
> - Creazione di un database.
> - Come connettersi a un database di.
> - Come visualizzare i dati in una pagina Web.
> - Come inserire, aggiornare ed eliminare i record del database.
> 
> Queste sono le funzionalità introdotte in questo articolo:
> 
> - Utilizzo di un database Microsoft SQL Server Compact Edition.
> - Utilizzo di query SQL.
> - Classe `Database`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Questa esercitazione funziona anche con WebMatrix 3. È possibile utilizzare Pagine Web ASP.NET 3 e Visual Studio 2013 (oppure Visual Studio Express 2013 per il Web); Tuttavia, l'interfaccia utente sarà diversa.

## <a name="introduction-to-databases"></a>Introduzione ai database

Immaginate una tipica Rubrica. Per ogni voce della Rubrica, ovvero per ogni persona, sono disponibili diverse informazioni, ad esempio il nome, il cognome, l'indirizzo, l'indirizzo di posta elettronica e il numero di telefono.

Un modo tipico per illustrare i dati come questo è una tabella con righe e colonne. In termini di database, ogni riga viene spesso definita record. Ogni colonna (talvolta denominata campi) contiene un valore per ogni tipo di dati: nome, cognome e così via.

| **ID** | **FirstName** | **LastName** | **Address** | **Posta elettronica** | **Telefono** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100 s SE orche WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Per la maggior parte delle tabelle di database, la tabella deve includere una colonna contenente un identificatore univoco, ad esempio un numero di cliente, un numero di conto e così via. Questa operazione è nota come *chiave primaria*della tabella e viene utilizzata per identificare ogni riga della tabella. Nell'esempio, la colonna ID è la chiave primaria per la Rubrica.

Con questa conoscenza di base dei database, si è pronti per apprendere come creare un database semplice ed eseguire operazioni quali l'aggiunta, la modifica e l'eliminazione di dati.

> [!TIP] 
> 
> **Database relazionali**
> 
> È possibile archiviare i dati in molti modi, inclusi file di testo e fogli di calcolo. Per la maggior parte degli usi aziendali, tuttavia, i dati vengono archiviati in un database relazionale.
> 
> Questo articolo non approfondisce i database. Tuttavia, può risultare utile comprenderne alcune informazioni. In un database relazionale le informazioni sono divise logicamente in tabelle separate. Ad esempio, un database per una scuola potrebbe contenere tabelle separate per gli studenti e per le offerte di classe. Il software del database, ad esempio SQL Server, supporta potenti comandi che consentono di stabilire in modo dinamico le relazioni tra le tabelle. È ad esempio possibile utilizzare il database relazionale per stabilire una relazione logica tra studenti e classi allo scopo di creare una pianificazione. L'archiviazione di dati in tabelle separate riduce la complessità della struttura della tabella e riduce la necessità di conservare i dati ridondanti nelle tabelle.

## <a name="creating-a-database"></a>Creazione di un database

Questa procedura illustra come creare un database denominato SmallBakery usando lo strumento di progettazione SQL Server Compact database incluso in WebMatrix. Sebbene sia possibile creare un database usando il codice, è più tipico creare il database e le tabelle di database usando uno strumento di progettazione come WebMatrix.

1. Avviare WebMatrix e nella pagina Avvio rapido fare clic su **sito da modello**.
2. Selezionare **sito vuoto**e nella casella **nome sito** immettere "SmallBakery" e quindi fare clic su **OK**. Il sito viene creato e visualizzato in WebMatrix.
3. Nel riquadro sinistro fare clic sull'area di lavoro **database** .
4. Sulla barra multifunzione fare clic su **nuovo database**. Viene creato un database vuoto con lo stesso nome del sito.
5. Nel riquadro sinistro espandere il nodo **SmallBakery. sdf** , quindi fare clic su **tabelle**.
6. Sulla barra multifunzione fare clic su **nuova tabella**. WebMatrix apre Progettazione tabelle.

    ![[immagine]](5-working-with-data/_static/image1.jpg)
7. Fare clic nella colonna **nome** e immettere Id &quot;&quot;.
8. Nella colonna **tipo di dati** selezionare **int**.
9. Impostare la **chiave primaria?** ed **è identificare?** opzioni su **Sì**.

    Come suggerisce il nome, la **chiave primaria** indica al database che questa sarà la chiave primaria della tabella. **Identity** indica al database di creare automaticamente un numero di ID per ogni nuovo record e di assegnargli il successivo numero sequenziale (a partire da 1).
10. Fare clic nella riga successiva. L'editor avvia una nuova definizione di colonna.
11. Per il valore nome immettere &quot;nome&quot;.
12. Per **tipo di dati**, scegliere &quot;nvarchar&quot; e impostare la lunghezza su 50. La parte *var* di `nvarchar` indica al database che i dati per questa colonna saranno una stringa le cui dimensioni possono variare da record a record. Il prefisso *n* rappresenta il *cittadino*, a indicare che il campo può contenere dati di tipo carattere che rappresentano qualsiasi alfabeto &#8212; o sistema di scrittura, ovvero che il campo contenga dati Unicode.
13. Impostare l'opzione **Consenti valori null** su **No**. Questa operazione impone che la colonna *Name* non venga lasciata vuota.
14. Utilizzando lo stesso processo, creare una colonna denominata *Description*. Impostare il **tipo di dati** su "nvarchar" e 50 per la lunghezza e impostare **Consenti valori null** su false.
15. Creare una colonna denominata *Price*. Impostare il **tipo di dati su "Money"** e impostare **Consenti valori null** su false.
16. Nella casella nella parte superiore assegnare un nome alla tabella &quot;&quot;del prodotto.

    Al termine, la definizione sarà simile alla seguente:

    ![[immagine]](5-working-with-data/_static/image2.png)
17. Premere CTRL + S per salvare la tabella.

## <a name="adding-data-to-the-database"></a>Aggiunta di dati al database

A questo punto è possibile aggiungere alcuni dati di esempio al database che verranno usati più avanti in questo articolo.

1. Nel riquadro sinistro espandere il nodo **SmallBakery. sdf** , quindi fare clic su **tabelle**.
2. Fare clic con il pulsante destro del mouse sulla tabella Product, quindi scegliere **dati**.
3. Nel riquadro modifica immettere i record seguenti:

    | **Name** | **Descrizione** | **Prezzo** |
    | --- | --- | --- |
    | Pane | Sfornato fresco ogni giorno. | 2.99 |
    | Torta fragola | Con fragole organiche dal nostro giardino. | 9.99 |
    | Torta Apple | Secondo solo per la torta di MOM. | 12.99 |
    | Torta pecan | Se ti piace pecan, questo è tutto per te. | 10.99 |
    | Torta limone | Realizzato con i migliori limoni al mondo. | 11.99 |
    | Tortine | I tuoi figli e il tuo ragazzo si ameranno. | 7.99 |

    Tenere presente che non è necessario immettere alcun valore per la colonna *ID* . Quando è stata creata la colonna *ID* , impostare la relativa proprietà **is Identity** su true, in modo che venga compilata automaticamente.

    Al termine dell'immissione dei dati, Progettazione tabelle sarà simile al seguente:

    ![[immagine]](5-working-with-data/_static/image3.jpg)
4. Chiudere la scheda contenente i dati del database.

## <a name="displaying-data-from-a-database"></a>Visualizzazione di dati da un database

Una volta ottenuto un database contenente dati, è possibile visualizzare i dati in una pagina Web ASP.NET. Per selezionare le righe della tabella da visualizzare, utilizzare un'istruzione SQL, ovvero un comando che viene passato al database.

1. Nel riquadro sinistro fare clic sull'area di lavoro **file** .
2. Nella directory principale del sito Web creare una nuova pagina CSHTML denominata *ListProducts. cshtml*.
3. Sostituire il markup esistente con il codice seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    Nel primo blocco di codice, aprire il file *SmallBakery. sdf* (database) creato in precedenza. Il metodo `Database.Open` presuppone che il file con *estensione sdf* si trovi nella cartella *app\_data* del sito Web. (Si noti che non è necessario specificare l' estensione &#8212; SDF, in caso contrario, il metodo `Open` non funzionerà).

    > [!NOTE]
    > La cartella *App\_data* è una cartella speciale in ASP.NET usata per archiviare i file di dati. Per ulteriori informazioni, vedere [connessione a un database](#SB_ConnectingToADatabase) più avanti in questo articolo.

    È quindi necessario eseguire una richiesta per eseguire una query sul database utilizzando l'istruzione SQL `Select` seguente:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    Nell'istruzione `Product` identifica la tabella in cui eseguire la query. Il carattere `*` specifica che la query deve restituire tutte le colonne della tabella. È anche possibile elencare le colonne singolarmente, separate da virgole, se si desidera visualizzare solo alcune colonne. La clausola `Order By` indica il modo in cui i dati &#8212; devono essere ordinati in questo caso dalla colonna *nome* . Ciò significa che i dati vengono ordinati alfabeticamente in base al valore della colonna *Name* per ogni riga.

    Nel corpo della pagina, il markup crea una tabella HTML che verrà usata per visualizzare i dati. All'interno dell'elemento `<tbody>` si usa un ciclo `foreach` per ottenere singolarmente ogni riga di dati restituita dalla query. Per ogni riga di dati, viene creata una riga della tabella HTML (elemento`<tr>`). Si creeranno quindi le celle della tabella HTML (elementi`<td>`) per ogni colonna. Ogni volta che si passa attraverso il ciclo, la successiva riga disponibile dal database si trova nella variabile `row` (questo viene configurato nell'istruzione `foreach`). Per ottenere una singola colonna dalla riga, è possibile usare `row.Name` o `row.Description` o qualunque sia il nome della colonna desiderata.
4. Eseguire la pagina in un browser. Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla. Nella pagina viene visualizzato un elenco simile al seguente:

    ![[immagine]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL è un linguaggio usato nella maggior parte dei database relazionali per la gestione dei dati in un database. Sono inclusi i comandi che consentono di recuperare i dati e di aggiornarli e che consentono di creare, modificare e gestire le tabelle di database. SQL è diverso rispetto a un linguaggio di programmazione (come quello che si sta usando in WebMatrix) perché con SQL, l'idea è quella di indicare al database quello che si vuole ed è il processo del database per capire come ottenere i dati o eseguire l'attività. Di seguito sono riportati alcuni esempi di comandi SQL e le operazioni eseguite:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> In questo modo vengono recuperate le colonne *ID*, *Name*e *Price* dai record della tabella *Product* se il valore di *Price* è maggiore di 10 e i risultati vengono restituiti in ordine alfabetico in base ai valori della colonna *Name* . Questo comando restituirà un set di risultati che contiene i record che soddisfano i criteri oppure un set vuoto se non è presente alcun record corrispondente.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> In questo modo viene inserito un nuovo record nella tabella *Product* , impostando la colonna *Name* su &quot;&quot;croissant, la colonna *Description* per &quot;un&quot;Scinto di fiocchi e il prezzo 1,99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Questo comando Elimina i record nella tabella *Product* la cui colonna Data di scadenza è precedente all'1 gennaio 2008. Si presuppone che la tabella *Product* includa tale colonna, ovviamente. La data viene immessa nel formato MM/gg/aaaa, ma deve essere immessa nel formato usato per le impostazioni locali.
> 
> I comandi `Insert Into` e `Delete` non restituiscono set di risultati. Restituiscono invece un numero che indica il numero di record interessati dal comando.
> 
> Per alcune di queste operazioni (ad esempio l'inserimento e l'eliminazione di record), il processo che richiede l'operazione deve disporre delle autorizzazioni appropriate nel database. Questo è il motivo per cui per i database di produzione spesso è necessario fornire un nome utente e una password quando ci si connette al database.
> 
> Sono disponibili dozzine di comandi SQL, ma tutti seguono un modello come questo. È possibile utilizzare i comandi SQL per creare tabelle di database, contare il numero di record in una tabella, calcolare i prezzi ed eseguire molte altre operazioni.

## <a name="inserting-data-in-a-database"></a>Inserimento di dati in un database

In questa sezione viene illustrato come creare una pagina che consente agli utenti di aggiungere un nuovo prodotto alla tabella del database del *prodotto* . Dopo l'inserimento di un nuovo record di prodotto, la pagina Visualizza la tabella aggiornata usando la pagina *ListProducts. cshtml* creata nella sezione precedente.

La pagina include la convalida per assicurarsi che i dati immessi dall'utente siano validi per il database. Il codice della pagina, ad esempio, assicura che sia stato immesso un valore per tutte le colonne obbligatorie.

1. Nel sito Web creare un nuovo file CSHTML denominato *InsertProducts. cshtml*.
2. Sostituire il markup esistente con il codice seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Il corpo della pagina contiene un form HTML con tre caselle di testo che consentono agli utenti di immettere un nome, una descrizione e un prezzo. Quando gli utenti fanno clic sul pulsante **Inserisci** , il codice nella parte superiore della pagina apre una connessione al database *SmallBakery. sdf* . Si ottengono quindi i valori che l'utente ha inviato usando l'oggetto `Request` e si assegnano tali valori alle variabili locali.

    Per verificare che l'utente abbia immesso un valore per ogni colonna obbligatoria, registrare ogni elemento `<input>` che si desidera convalidare:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    Il `Validation` Helper verifica che esista un valore in ogni campo registrato. È possibile verificare se tutti i campi hanno superato la convalida controllando `Validation.IsValid()`, che in genere si esegue prima di elaborare le informazioni che si ottengono dall'utente:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    L'operatore `&&` indica e: questo test è se si tratta *di un invio di form e tutti i campi hanno superato la convalida*.

    Se tutte le colonne sono state convalidate (nessuna è vuota), creare un'istruzione SQL per inserire i dati e quindi eseguirli come illustrato di seguito:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Per i valori da inserire, includere i segnaposto dei parametri (`@0`, `@1``@2`).

    > [!NOTE]
    > Come precauzione di sicurezza, passare sempre i valori a un'istruzione SQL usando i parametri, come illustrato nell'esempio precedente. In questo modo è possibile convalidare i dati dell'utente, oltre a garantire la protezione da tentativi di invio di comandi dannosi al database (talvolta definiti attacchi intrusivi nel codice SQL).

    Per eseguire la query, usare questa istruzione, passando a essa le variabili contenenti i valori da sostituire ai segnaposto:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Al termine dell'esecuzione dell'istruzione `Insert Into`, l'utente viene inviato alla pagina in cui sono elencati i prodotti che usano questa riga:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Se la convalida ha esito negativo, si ignora l'inserimento. È invece disponibile un helper nella pagina in cui è possibile visualizzare i messaggi di errore accumulati, se presenti:

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Si noti che il blocco di stile nel markup include una definizione di classe CSS denominata `.validation-summary-errors`. Si tratta del nome della classe CSS utilizzata per impostazione predefinita per l'elemento `<div>` che contiene gli errori di convalida. In questo caso, la classe CSS specifica che gli errori di riepilogo della convalida vengono visualizzati in rosso e in grassetto, ma è possibile definire la classe `.validation-summary-errors` per visualizzare le eventuali formattazioni desiderate.

### <a name="testing-the-insert-page"></a>Test della pagina di inserimento

1. Visualizzare la pagina in un browser. Nella pagina viene visualizzato un modulo simile a quello illustrato nella figura seguente.

    ![[immagine]](5-working-with-data/_static/image5.jpg)
2. Immettere i valori per tutte le colonne, ma assicurarsi di lasciare vuota la colonna *Price* .
3. Fare clic su **Inserisci**. Nella pagina viene visualizzato un messaggio di errore, come illustrato nella figura seguente. Non viene creato alcun nuovo record.

    ![[immagine]](5-working-with-data/_static/image6.jpg)
4. Compilare completamente il modulo, quindi fare clic su **Inserisci**. Questa volta, viene visualizzata la pagina *ListProducts. cshtml* che mostra il nuovo record.

## <a name="updating-data-in-a-database"></a>Aggiornamento di dati in un database

Dopo aver immesso i dati in una tabella, potrebbe essere necessario aggiornarli. Questa procedura illustra come creare due pagine simili a quelle create per l'inserimento dei dati in precedenza. Nella prima pagina vengono visualizzati i prodotti che consentono agli utenti di selezionarne uno da modificare. La seconda pagina consente agli utenti di apportare le modifiche e salvarle.

> [!NOTE] 
> 
> **Importante** In un sito Web di produzione, in genere si limitano gli utenti autorizzati a apportare modifiche ai dati. Per informazioni su come configurare l'appartenenza e su come autorizzare gli utenti a eseguire attività sul sito, vedere [aggiunta di sicurezza e appartenenza a un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).

1. Nel sito Web creare un nuovo file CSHTML denominato *EditProducts. cshtml*.
2. Sostituire il markup esistente nel file con il codice seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    L'unica differenza tra questa pagina e la pagina *ListProducts. cshtml* precedente è che la tabella HTML in questa pagina include una colonna aggiuntiva che visualizza un collegamento di **modifica** . Quando si fa clic su questo collegamento, si passa alla pagina *UpdateProducts. cshtml* (che verrà creata successivamente) in cui è possibile modificare il record selezionato.

    Esaminare il codice che crea il collegamento **Edit (modifica** ):

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Viene creato un elemento `<a>` HTML il cui attributo `href` è impostato in modo dinamico. L'attributo `href` specifica la pagina da visualizzare quando l'utente fa clic sul collegamento. Passa anche il valore `Id` della riga corrente al collegamento. Quando la pagina viene eseguita, l'origine della pagina potrebbe contenere collegamenti simili ai seguenti:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Si noti che l'attributo `href` è impostato su `UpdateProducts/n`, dove *n* è un numero di prodotto. Quando un utente fa clic su uno di questi collegamenti, l'URL risultante sarà simile al seguente:

    `http://localhost:18816/UpdateProducts/6`

    In altre parole, il numero di prodotto da modificare verrà passato nell'URL.
3. Visualizzare la pagina in un browser. La pagina Visualizza i dati in un formato simile al seguente:

    ![[immagine]](5-working-with-data/_static/image7.jpg)

    Successivamente, verrà creata la pagina che consente agli utenti di aggiornare effettivamente i dati. La pagina di aggiornamento include la convalida per convalidare i dati immessi dall'utente. Il codice della pagina, ad esempio, assicura che sia stato immesso un valore per tutte le colonne obbligatorie.
4. Nel sito Web creare un nuovo file CSHTML denominato *UpdateProducts. cshtml*.
5. Sostituire il markup esistente nel file con il codice seguente.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Il corpo della pagina contiene un form HTML in cui viene visualizzato un prodotto che può essere modificato dagli utenti. Per visualizzare il prodotto, usare questa istruzione SQL:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Verrà selezionato il prodotto il cui ID corrisponde al valore passato nel parametro `@0`. Poiché *ID* è la chiave primaria e pertanto deve essere univoco, è possibile selezionare un solo record di prodotto in questo modo. Per ottenere il valore ID da passare a questa istruzione `Select`, è possibile leggere il valore passato alla pagina come parte dell'URL, usando la sintassi seguente:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Per recuperare effettivamente il record del prodotto, usare il metodo `QuerySingle`, che restituirà solo un record:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    La riga singola viene restituita nella variabile `row`. È possibile ottenere i dati da ogni colonna e assegnarli alle variabili locali come segue:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Nel markup per il form, questi valori vengono visualizzati automaticamente nelle singole caselle di testo utilizzando codice incorporato simile al seguente:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Tale parte del codice consente di visualizzare il record del prodotto da aggiornare. Una volta visualizzato il record, l'utente può modificare le singole colonne.

    Quando l'utente invia il modulo facendo clic sul pulsante **Aggiorna** , viene eseguito il codice nel blocco `if(IsPost)`. Ciò consente di ottenere i valori dell'utente dall'oggetto `Request`, di archiviare i valori nelle variabili e di verificare che ogni colonna sia stata compilata. Se la convalida viene passata, il codice crea l'istruzione SQL Update seguente:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    In un'istruzione SQL `Update` specificare ogni colonna da aggiornare e il valore in cui impostarla. In questo codice, i valori vengono specificati usando i segnaposto dei parametri `@0`, `@1`, `@2`e così via. Come indicato in precedenza, per la sicurezza, è necessario passare sempre i valori a un'istruzione SQL usando i parametri.

    Quando si chiama il metodo `db.Execute`, si passano le variabili che contengono i valori nell'ordine che corrisponde ai parametri nell'istruzione SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Dopo l'esecuzione dell'istruzione `Update`, chiamare il metodo seguente per reindirizzare l'utente alla pagina di modifica:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    L'effetto è che l'utente visualizza un elenco aggiornato dei dati nel database e può modificare un altro prodotto.
6. Salvare la pagina.
7. Eseguire la pagina *EditProducts. cshtml* (non la pagina aggiornamento), quindi fare clic su **modifica** per selezionare un prodotto da modificare. Viene visualizzata la pagina *UpdateProducts. cshtml* che mostra il record selezionato.

    ![[immagine]](5-working-with-data/_static/image8.jpg)
8. Apportare una modifica e fare clic su **Aggiorna**. L'elenco Products viene nuovamente visualizzato con i dati aggiornati.

## <a name="deleting-data-in-a-database"></a>Eliminazione di dati in un database

In questa sezione viene illustrato come consentire agli utenti di eliminare un prodotto dalla tabella di database del *prodotto* . L'esempio è costituito da due pagine. Nella prima pagina gli utenti selezionano un record da eliminare. Il record da eliminare viene quindi visualizzato in una seconda pagina che consente di confermare che desidera eliminare il record.

> [!NOTE] 
> 
> **Importante** In un sito Web di produzione, in genere si limitano gli utenti autorizzati a apportare modifiche ai dati. Per informazioni su come configurare l'appartenenza e su come autorizzare l'utente a eseguire attività nel sito, vedere [aggiunta di sicurezza e appartenenza a un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).

1. Nel sito Web creare un nuovo file CSHTML denominato *ListProductsForDelete. cshtml*.
2. Sostituire il markup esistente con il codice seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Questa pagina è simile alla pagina *EditProducts. cshtml* precedente. Tuttavia, anziché visualizzare un collegamento di **modifica** per ogni prodotto, viene visualizzato un collegamento **Delete** . Il collegamento **Delete** viene creato usando il codice incorporato seguente nel markup:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    In questo modo viene creato un URL simile al seguente quando gli utenti fanno clic sul collegamento:

    `http://<server>/DeleteProduct/4`

    L'URL chiama una pagina denominata *DeleteProduct. cshtml* (che verrà creata successivamente) e la passa all'ID del prodotto da eliminare (in questo caso 4).
3. Salvare il file, ma lasciarlo aperto.
4. Creare un altro file CHTML denominato *DeleteProduct. cshtml*. Sostituire il contenuto esistente con il codice seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Questa pagina viene chiamata da *ListProductsForDelete. cshtml* e consente agli utenti di confermare l'eliminazione di un prodotto. Per elencare il prodotto da eliminare, ottenere l'ID del prodotto da eliminare dall'URL usando il codice seguente:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    La pagina chiede quindi all'utente di fare clic su un pulsante per eliminare effettivamente il record. Si tratta di una misura di sicurezza importante: quando si eseguono operazioni sensibili nel sito Web, ad esempio l'aggiornamento o l'eliminazione di dati, queste operazioni devono essere eseguite sempre usando un'operazione POST, non un'operazione GET. Se il sito è configurato in modo che un'operazione di eliminazione possa essere eseguita usando un'operazione GET, chiunque può passare un URL come `http://<server>/DeleteProduct/4` ed eliminare qualsiasi elemento desiderato dal database. Aggiungendo la conferma e codificando la pagina in modo che l'eliminazione possa essere eseguita solo tramite un POST, si aggiunge una misura di sicurezza al sito.

    L'operazione di eliminazione effettiva viene eseguita usando il codice seguente, che conferma innanzitutto che si tratta di un'operazione post e che l'ID non è vuoto:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Il codice esegue un'istruzione SQL che elimina il record specificato e quindi reindirizza l'utente alla pagina di elenco.
5. Eseguire *ListProductsForDelete. cshtml* in un browser.

    ![[immagine]](5-working-with-data/_static/image9.jpg)
6. Fare clic sul collegamento **Elimina** per uno dei prodotti. Viene visualizzata la pagina *DeleteProduct. cshtml* per confermare che si vuole eliminare il record.
7. Fare clic sul pulsante **Elimina**. Il record del prodotto viene eliminato e la pagina viene aggiornata con una lista di prodotti aggiornata.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Connessione a un database
> 
> È possibile connettersi a un database in due modi. Il primo consiste nell'usare il metodo `Database.Open` e specificare il nome del file di database (meno l'estensione *SDF* ):
> 
> `var db = Database.Open("SmallBakery");`
> 
> Il metodo `Open` presuppone che. il file *SDF* si trova nella cartella *app\_data* del sito Web. Questa cartella è progettata in modo specifico per contenere i dati. Ad esempio, ha le autorizzazioni appropriate per consentire al sito Web di leggere e scrivere dati e, come misura di sicurezza, WebMatrix non consente l'accesso ai file da questa cartella.
> 
> Il secondo modo consiste nell'usare una stringa di connessione. Una stringa di connessione contiene informazioni sulla connessione a un database. Può includere un percorso di file oppure può includere il nome di un database di SQL Server in un server locale o remoto, insieme a un nome utente e una password per connettersi a tale server. Se si mantengono i dati in una versione gestita a livello centralizzato di SQL Server, ad esempio nel sito di un provider di hosting, è sempre necessario utilizzare una stringa di connessione per specificare le informazioni di connessione al database.
> 
> In WebMatrix le stringhe di connessione vengono in genere archiviate in un file XML denominato *Web. config*. Come suggerisce il nome, è possibile usare un file *Web. config* nella radice del sito Web per archiviare le informazioni di configurazione del sito, incluse eventuali stringhe di connessione che il sito potrebbe richiedere. Un esempio di una stringa di connessione in un file *Web. config* potrebbe essere simile al seguente:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> Nell'esempio, la stringa di connessione punta a un database in un'istanza di SQL Server eseguita in un server in un punto qualsiasi, anziché in un file *SDF* locale. È necessario sostituire i nomi appropriati per `myServer` e `myDatabase`e specificare SQL Server i valori di accesso per `username` e `password`. I valori di nome utente e password non sono necessariamente identici alle credenziali di Windows o ai valori che il provider di hosting ha fornito per l'accesso ai server. Rivolgersi all'amministratore per individuare i valori esatti necessari.
> 
> Il metodo `Database.Open` è flessibile, in quanto consente di passare il nome di un file con estensione *SDF* del database o il nome di una stringa di connessione archiviata nel file *Web. config* . Nell'esempio seguente viene illustrato come connettersi al database utilizzando la stringa di connessione illustrata nell'esempio precedente:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Come indicato, il metodo `Database.Open` consente di passare un nome di database o una stringa di connessione e scoprire quale usare. Questa operazione è molto utile quando si distribuisce (pubblica) il sito Web. È possibile usare un file *SDF* nella cartella *app\_data* durante lo sviluppo e il test del sito. Quando si sposta il sito in un server di produzione, è possibile utilizzare una stringa di connessione nel file *Web. config* con lo stesso nome del file *SDF* ma che punta al database &#8212; del provider di hosting senza dover modificare il codice.
> 
> Infine, se si desidera lavorare direttamente con una stringa di connessione, è possibile chiamare il metodo `Database.OpenConnectionString` e passargli la stringa di connessione effettiva anziché solo il nome di uno nel file *Web. config* . Questo può essere utile nelle situazioni in cui per qualche motivo non è possibile accedere alla stringa di connessione (o ai valori in esso contenuti, ad esempio il nome del file con *estensione sdf* ) finché la pagina non è in esecuzione. Per la maggior parte degli scenari, tuttavia, è possibile usare `Database.Open` come descritto in questo articolo.

## <a name="additional-resources"></a>Risorse aggiuntive

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Connessione a un database SQL Server o MySQL in WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Convalida dell'input utente nelle pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
