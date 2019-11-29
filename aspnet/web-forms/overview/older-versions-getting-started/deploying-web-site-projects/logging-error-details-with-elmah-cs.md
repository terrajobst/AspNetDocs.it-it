---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: Registrazione dei dettagli degli errori conC#ELMAH () | Microsoft Docs
author: rick-anderson
description: La registrazione degli errori di moduli e gestori (ELMAH) offre un altro approccio alla registrazione degli errori di runtime in un ambiente di produzione. ELMAH è un errore open source gratuito...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: 5018023eced23e7a70eab90e649f85862c548940
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74570410"
---
# <a name="logging-error-details-with-elmah-c"></a>Registrazione dei dettagli degli errori con ELMAH (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> La registrazione degli errori di moduli e gestori (ELMAH) offre un altro approccio alla registrazione degli errori di runtime in un ambiente di produzione. ELMAH è una libreria di registrazione degli errori open source gratuita che include funzionalità come il filtro degli errori e la possibilità di visualizzare il log degli errori da una pagina Web, come feed RSS, o di scaricarlo come file delimitato da virgole. Questa esercitazione illustra come scaricare e configurare ELMAH.

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](logging-error-details-with-asp-net-health-monitoring-cs.md) è stato esaminato ASP. Sistema di monitoraggio dell'integrità di NET, che offre una libreria predefinita per la registrazione di un'ampia gamma di eventi Web. Molti sviluppatori utilizzano il monitoraggio dello stato per registrare e inviare tramite posta elettronica i dettagli delle eccezioni non gestite. Tuttavia, esistono alcuni punti deboli con questo sistema. In primo luogo, è la mancanza di un'interfaccia utente per la visualizzazione di informazioni sugli eventi registrati. Se si desidera visualizzare un riepilogo dei 10 ultimi errori o visualizzare i dettagli di un errore che si è verificato la settimana scorsa, è necessario esaminare il database, analizzare la posta in arrivo o creare una pagina Web che visualizzi le informazioni dalla tabella `aspnet_WebEvent_Events`.

Un altro punto problematico si concentra sulla complessità del monitoraggio dell'integrità. Poiché il monitoraggio dello stato può essere utilizzato per registrare una pletora di eventi diversi e, poiché esistono diverse opzioni per indicare come e quando gli eventi vengono registrati, la configurazione corretta del sistema di monitoraggio dello stato può essere un'attività gravosa. Infine, esistono problemi di compatibilità. Poiché il monitoraggio dell'integrità è stato aggiunto per la prima volta alla .NET Framework nella versione 2,0, non è disponibile per le applicazioni Web precedenti compilate con ASP.NET versione 1. x. Inoltre, la classe `SqlWebEventProvider`, che è stata usata nell'esercitazione precedente per registrare i dettagli dell'errore in un database, funziona solo con Microsoft SQL Server database. È necessario creare una classe di provider di log personalizzata se è necessario registrare gli errori in un archivio dati alternativo, ad esempio un file XML o un database Oracle.

Un'alternativa al sistema di monitoraggio dello stato è la registrazione degli errori dei moduli e dei gestori (ELMAH), un [sistema di registrazione degli errori](http://www.raboof.com/)open source gratuito creato da. La differenza più significativa tra i due sistemi è la capacità di ELAMH di visualizzare un elenco di errori e i dettagli di un errore specifico da una pagina Web e come feed RSS. ELMAH è più facile da configurare rispetto al monitoraggio dell'integrità perché registra solo gli errori. Inoltre, ELMAH include il supporto per le applicazioni ASP.NET 1. x, ASP.NET 2,0 e ASP.NET 3,5 e viene fornito con un'ampia gamma di provider di origine dei log.

Questa esercitazione illustra i passaggi necessari per l'aggiunta di ELMAH a un'applicazione ASP.NET. Iniziamo!

> [!NOTE]
> Il sistema di monitoraggio dell'integrità e ELMAH hanno entrambi set di vantaggi e svantaggi. Si consiglia di provare entrambi i sistemi e di decidere quale sia la soluzione più adatta alle proprie esigenze.

## <a name="adding-elmah-to-an-aspnet-web-application"></a>Aggiunta di ELMAH a un'applicazione Web ASP.NET

L'integrazione di ELMAH in un'applicazione ASP.NET nuova o esistente è un processo semplice e diretto che richiede meno di cinque minuti. In breve, comporta quattro semplici passaggi:

1. Scaricare ELMAH e aggiungere l'assembly `Elmah.dll` all'applicazione Web.
2. Registrare i moduli e il gestore HTTP di ELMAH in `Web.config`,
3. Specificare le opzioni di configurazione di ELMAH e
4. Creare l'infrastruttura di origine dei log degli errori, se necessario.

Esaminiamo ognuno di questi quattro passaggi, uno alla volta.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Passaggio 1: scaricare i file di progetto ELMAH e aggiungere`Elmah.dll`all'applicazione Web

ELMAH 1,0 BETA 3 (Build 10617), la versione più recente al momento della stesura di questo articolo, è incluso nel download disponibile in questa esercitazione. In alternativa, è possibile visitare il [sito Web ELMAH](https://code.google.com/p/elmah/) per ottenere la versione più recente o scaricare il codice sorgente. Estrarre il download di ELMAH in una cartella sul desktop e individuare il file di assembly ELMAH (`Elmah.dll`).

> [!NOTE]
> Il file di `Elmah.dll` si trova nella cartella `Bin` download, che include sottocartelle per diverse versioni di .NET Framework e per le compilazioni di rilascio e debug. Usare la build di rilascio per la versione del Framework appropriata. Ad esempio, se si compila un'applicazione Web ASP.NET 3,5, copiare il file `Elmah.dll` dalla cartella `Bin\net-3.5\Release`.

Successivamente, aprire Visual Studio e aggiungere l'assembly al progetto facendo clic con il pulsante destro del mouse sul nome del sito Web nella Esplora soluzioni e scegliendo Aggiungi riferimento dal menu di scelta rapida. Verrà visualizzata la finestra di dialogo Aggiungi riferimento. Passare alla scheda Sfoglia e scegliere il file di `Elmah.dll`. Questa azione aggiunge il file di `Elmah.dll` alla cartella `Bin` dell'applicazione Web.

> [!NOTE]
> Il tipo di progetto di applicazione Web (WAP) non Mostra la cartella `Bin` nel Esplora soluzioni. Elenca invece questi elementi nella cartella riferimenti.

L'assembly `Elmah.dll` include le classi utilizzate dal sistema ELMAH. Queste classi rientrano in una delle tre categorie seguenti:

- **Moduli HTTP** : un modulo HTTP è una classe che definisce i gestori eventi per `HttpApplication` eventi, ad esempio `Error` evento. ELMAH include più moduli HTTP, tre dei quali sono: 

    - `ErrorLogModule`: registra le eccezioni non gestite in un'origine del log.
    - `ErrorMailModule`: Invia i dettagli di un'eccezione non gestita in un messaggio di posta elettronica.
    - `ErrorFilterModule`: applica i filtri specificati dallo sviluppatore per determinare quali eccezioni vengono registrate e quali verranno ignorate.
- **Gestori HTTP** : un gestore HTTP è una classe responsabile della generazione del markup per un tipo specifico di richiesta. ELMAH include gestori HTTP che eseguono il rendering dei dettagli dell'errore come pagina Web, come feed RSS o come file con valori delimitati da virgole (CSV).
- **Origini dei log degli errori** : il ELMAH è in grado di registrare gli errori in memoria, in un database di Microsoft SQL Server, in un database di Microsoft Access, in un database Oracle, in un file XML, in un database SQLite o in un database di database vista. Come il sistema di monitoraggio dello stato, l'architettura di ELMAH è stata creata usando il modello di provider, pertanto è possibile creare e integrare facilmente i provider di origine dei log personalizzati, se necessario.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Passaggio 2: registrare il modulo e il gestore HTTP di ELMAH

Sebbene il file di `Elmah.dll` contenga i moduli e il gestore HTTP necessari per registrare automaticamente le eccezioni non gestite e visualizzare i dettagli dell'errore da una pagina Web, è necessario registrarli in modo esplicito nella configurazione dell'applicazione Web. Il `ErrorLogModule` modulo HTTP, una volta registrato, sottoscrive l'evento di `Error` del `HttpApplication`. Ogni volta che viene generato questo evento, il `ErrorLogModule` registra i dettagli dell'eccezione in un'origine di log specificata. Si vedrà come definire il provider di origine dei log nella sezione successiva, "Configuring ELMAH". Il `ErrorLogPageFactory` Factory del gestore HTTP è responsabile della generazione del markup durante la visualizzazione del log degli errori da una pagina Web.

La sintassi specifica per la registrazione di moduli e gestori HTTP dipende dal server Web che sta alimentando il sito. Per la Server di sviluppo ASP.NET e Microsoft IIS versione 6,0 e versioni precedenti, i moduli e i gestori HTTP sono registrati nelle sezioni `<httpModules>` e `<httpHandlers>`, che vengono visualizzate all'interno dell'elemento `<system.web>`. Se si usa IIS 7,0, è necessario registrarli nelle sezioni `<modules>` e `<handlers>` dell'elemento `<system.webServer>`. Fortunatamente, è possibile definire i moduli e i gestori HTTP in *entrambe* le posizioni indipendentemente dal server Web in uso. Questa opzione è la più portabile, in quanto consente di usare la stessa configurazione negli ambienti di sviluppo e di produzione indipendentemente dal server Web in uso.

Per iniziare, registrare il modulo HTTP `ErrorLogModule` e il gestore HTTP `ErrorLogPageFactory` nella sezione `<httpModules>` e `<httpHandlers>` del `<system.web>`. Se la configurazione definisce già questi due elementi, è sufficiente includere l'elemento `<add>` per il modulo e il gestore HTTP di ELMAH.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

Registrare quindi il modulo e il gestore HTTP di ELMAH nell'elemento `<system.webServer>`. Come prima, se questo elemento non è già presente nella configurazione, aggiungerlo.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

Per impostazione predefinita, IIS 7 si lamenta se i moduli e i gestori HTTP sono registrati nella sezione `<system.web>`. L'attributo `validateIntegratedModeConfiguration` nell'elemento `<validation>` indica a IIS 7 di non visualizzare tali messaggi di errore.

Si noti che la sintassi per la registrazione del gestore HTTP `ErrorLogPageFactory` include un attributo `path`, che è impostato su `elmah.axd`. Questo attributo informa l'applicazione Web che se arriva una richiesta per una pagina denominata `elmah.axd`, la richiesta deve essere elaborata dal gestore HTTP `ErrorLogPageFactory`. Verrà visualizzato il `ErrorLogPageFactory` gestore HTTP in azione più avanti in questa esercitazione.

### <a name="step-3-configuring-elmah"></a>Passaggio 3: configurazione di ELMAH

ELMAH Cerca le opzioni di configurazione nel file di `Web.config` del sito Web in una sezione di configurazione personalizzata denominata `<elmah>`. Per usare una sezione personalizzata in `Web.config` è necessario prima definire l'elemento nell'elemento `<configSections>`. Aprire il file di `Web.config` e aggiungere il markup seguente al `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> Se si sta configurando ELMAH per un'applicazione ASP.NET 1. x, rimuovere l'attributo `requirePermission="false"` dagli elementi `<section>` precedenti.

La sintassi precedente registra la sezione `<elmah>` personalizzata e le relative sottosezioni: `<security>`, `<errorLog>`, `<errorMail>`e `<errorFilter>`.

Successivamente, aggiungere la sezione `<elmah>` `Web.config`. Questa sezione deve essere visualizzata allo stesso livello dell'elemento `<system.web>`. Nella sezione `<elmah>` aggiungere le sezioni `<security>` e `<errorLog>` come segue:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

L'attributo `allowRemoteAccess` della sezione `<security>` indica se è consentito l'accesso remoto. Se questo valore è impostato su 0, la pagina Web log degli errori può essere visualizzata solo localmente. Se questo attributo è impostato su 1, la pagina Web log degli errori è abilitata per i visitatori locali e remoti. Per il momento, disabilitare la pagina Web log degli errori per i visitatori remoti. Si consentirà l'accesso remoto in un secondo momento, dopo avere avuto la possibilità di discutere le problematiche relative alla sicurezza.

La sezione `<errorLog>` definisce l'origine del log degli errori, che determina la posizione in cui vengono registrati i dettagli dell'errore. è simile alla sezione `<providers>` del sistema di monitoraggio dello stato. La sintassi precedente specifica la classe `SqlErrorLog` come origine del log degli errori, che consente di registrare gli errori in un database Microsoft SQL Server specificato dal valore dell'attributo `connectionStringName`.

> [!NOTE]
> ELMAH viene fornito con provider di log degli errori aggiuntivi che possono essere utilizzati per registrare gli errori in un file XML, un database di Microsoft Access, un database Oracle e altri archivi dati. Per informazioni sull'uso di questi provider di log degli errori alternativi, vedere il file di `Web.config` di esempio incluso nel download di ELMAH.

### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Passaggio 4: creazione dell'infrastruttura di origine dei log degli errori

Il provider di `SqlErrorLog` di ELMAH registra i dettagli dell'errore in un database di Microsoft SQL Server specificato. Il provider di `SqlErrorLog` prevede che il database disponga di una tabella denominata `ELMAH_Error` e di tre stored procedure: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`e `ELMAH_LogError`. Il download di ELMAH include un file denominato `SQLServer.sql` nella cartella `db` che contiene l'istruzione T-SQL per la creazione di questa tabella e di queste stored procedure. È necessario eseguire queste istruzioni nel database per usare il provider di `SqlErrorLog`.

Le **figure 1** e **2** mostrano le Esplora database in Visual Studio dopo che sono stati aggiunti gli oggetti di database necessari per il provider di `SqlErrorLog`.

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**Figura 1**: il Provider di `SqlErrorLog` registra gli errori nella tabella `ELMAH_Error`

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**Figura 2**: il Provider di `SqlErrorLog` utilizza tre stored procedure

## <a name="elmah-in-action"></a>ELMAH in azione

A questo punto è stato aggiunto ELMAH all'applicazione Web, sono stati registrati il modulo HTTP `ErrorLogModule` e il gestore HTTP `ErrorLogPageFactory`, sono state specificate le opzioni di configurazione di ELMAH in `Web.config`e sono stati aggiunti gli oggetti di database necessari per il provider di log degli errori `SqlErrorLog`. A questo punto è possibile vedere ELMAH in azione. Visitare il sito Web revisioni e visitare una pagina che genera un errore di runtime, ad esempio `Genre.aspx?ID=foo`o una pagina inesistente, ad esempio `NoSuchPage.aspx`. Gli elementi visualizzati quando si visitano queste pagine dipendono dalla configurazione del `<customErrors>` e dall'eventuale visita in locale o in remoto. Per un aggiornamento in questo argomento, vedere l' [esercitazione *visualizzazione di una pagina di errore personalizzata* ](displaying-a-custom-error-page-cs.md) .

ELMAH non influisce sul contenuto visualizzato all'utente quando si verifica un'eccezione non gestita. registra semplicemente i dettagli. Questo log degli errori è accessibile dalla pagina Web `elmah.axd` dalla radice del sito Web, ad esempio `http://localhost/BookReviews/elmah.axd`. Questo file non esiste fisicamente nel progetto, ma quando viene ricevuta una richiesta per `elmah.axd` il runtime lo invia al gestore HTTP `ErrorLogPageFactory`, che genera il markup inviato al browser.

> [!NOTE]
> È anche possibile usare la pagina `elmah.axd` per indicare a ELMAH di generare un errore di test. Visitando `elmah.axd/test` (come in, `http://localhost/BookReviews/elmah.axd/test`) fa in modo che ELMAH generi un'eccezione di tipo `Elmah.TestException`, che contiene il messaggio di errore: "si tratta di un'eccezione di test che può essere ignorata in modo sicuro".

**Nella figura 3** viene illustrato il log degli errori quando si visita `elmah.axd` dall'ambiente di sviluppo.

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**Figura 3**: `Elmah.axd` Visualizza il log degli errori da una pagina Web  
([Fare clic per visualizzare l'immagine con dimensioni complete](logging-error-details-with-elmah-cs/_static/image7.png))

Il log degli errori nella **Figura 3** contiene sei voci di errore. Ogni voce include il codice di stato HTTP (404 o 500, per questi errori), il tipo, la descrizione, il nome dell'utente che ha eseguito l'accesso quando si è verificato l'errore e la data e l'ora. Se si fa clic sul collegamento dettagli, viene visualizzata una pagina che include lo stesso messaggio di errore visualizzato nella schermata gialla Dettagli errore (vedere la **Figura 4**) insieme ai valori delle variabili server al momento dell'errore (vedere la **Figura 5**). È inoltre possibile visualizzare il codice XML non elaborato in cui vengono salvati i dettagli dell'errore, incluse informazioni aggiuntive, ad esempio i valori nell'intestazione HTTP POST.

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**Figura 4**: visualizzare i dettagli dell'errore YSOD  
([Fare clic per visualizzare l'immagine con dimensioni complete](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**Figura 5**: esplorare i valori della raccolta di variabili server al momento dell'errore  
([Fare clic per visualizzare l'immagine con dimensioni complete](logging-error-details-with-elmah-cs/_static/image13.png))

La distribuzione di ELMAH nel sito Web di produzione comporta:

- Copia del file di `Elmah.dll` nella cartella `Bin` in produzione,
- Copiando le impostazioni di configurazione specifiche di ELMAH nel file `Web.config` usato in produzione e
- Aggiunta dell'infrastruttura di origine dei log degli errori al database di produzione.

Sono state analizzate le tecniche per la copia dei file dallo sviluppo alla produzione nelle esercitazioni precedenti. Probabilmente il modo più semplice per ottenere l'infrastruttura di origine dei log degli errori nel database di produzione consiste nell'usare SQL Server Management Studio per connettersi al database di produzione e quindi eseguire il file di script `SqlServer.sql`, che creerà la tabella e le stored procedure necessarie.

### <a name="viewing-the-error-details-page-on-production"></a>Visualizzazione della pagina dei dettagli dell'errore in produzione

Dopo aver distribuito il sito in produzione, visitare il sito Web di produzione e generare un'eccezione non gestita. Come nell'ambiente di sviluppo, ELMAH non ha alcun effetto sulla pagina di errore visualizzata quando si verifica un'eccezione non gestita. ma semplicemente registra l'errore. Se si tenta di visitare la pagina del log degli errori (`elmah.axd`) dall'ambiente di produzione, viene visualizzata la pagina non consentita mostrata nella **Figura 6**.

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**Figura 6**: per impostazione predefinita, i visitatori remoti non possono visualizzare la pagina Web del log degli errori  
([Fare clic per visualizzare l'immagine con dimensioni complete](logging-error-details-with-elmah-cs/_static/image16.png))

Si tenga presente che nella sezione `<security>` della configurazione di ELMAH si imposta l'attributo `allowRemoteAccess` su 0, che impedisce agli utenti remoti di visualizzare il log degli errori. È importante impedire ai visitatori anonimi di visualizzare il log degli errori, perché i dettagli dell'errore potrebbero rivelare vulnerabilità della sicurezza o altre informazioni riservate. Se si decide di impostare questo attributo su 1 e si Abilita l'accesso remoto al log degli errori, è importante bloccare il percorso di `elmah.axd` in modo che solo i visitatori autorizzati possano accedervi. Per ottenere questo risultato, è possibile aggiungere un elemento `<location>` al file di `Web.config`.

La configurazione seguente consente solo agli utenti con ruolo di amministratore di accedere alla pagina Web del log degli errori:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> Il ruolo di amministratore e i tre utenti del sistema, Scott, Jisun e Alice, sono stati aggiunti nell'esercitazione [ *configurazione di un sito Web che usa servizi per le applicazioni* ](configuring-a-website-that-uses-application-services-cs.md). Gli utenti Scott e Jisun sono membri del ruolo amministratore. Per ulteriori informazioni sull'autenticazione e l'autorizzazione, vedere le [esercitazioni sulla sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Il log degli errori sull'ambiente di produzione ora può essere visualizzato dagli utenti remoti. per gli screenshot della pagina Web log degli errori, fare riferimento alle **figure 3**, **4**e **5** . Tuttavia, se un utente anonimo o non amministratore tenta di visualizzare la pagina del log degli errori, viene automaticamente reindirizzato alla pagina di accesso (`Login.aspx`), come illustrato **nella figura 7** .

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**Figura 7**: gli utenti non autorizzati vengono reindirizzati automaticamente alla pagina di accesso  
([Fare clic per visualizzare l'immagine con dimensioni complete](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Registrazione a livello di codice degli errori

Il modulo HTTP `ErrorLogModule` di ELMAH registra automaticamente le eccezioni non gestite nell'origine di log specificata. In alternativa, è possibile registrare un errore senza dover generare un'eccezione non gestita usando la classe `ErrorSignal` e il relativo metodo `Raise`. Al metodo `Raise` viene passato un oggetto `Exception` e lo registra come se tale eccezione fosse stata generata e avesse raggiunto il runtime ASP.NET senza essere gestita. La differenza, tuttavia, è che la richiesta continua l'esecuzione normalmente dopo la chiamata del metodo `Raise`, mentre un'eccezione generata, non gestita interrompe l'esecuzione normale della richiesta e fa in modo che il runtime ASP.NET visualizzi la pagina di errore configurata.

La classe `ErrorSignal` è utile nelle situazioni in cui è presente un'azione che può avere esito negativo, ma l'errore non è irreversibile per l'operazione complessiva eseguita. Ad esempio, un sito Web può contenere un modulo che accetta l'input dell'utente, lo archivia in un database e quindi invia all'utente un messaggio di posta elettronica che informa che sono state elaborate le informazioni. Cosa dovrebbe succedere se le informazioni vengono salvate nel database correttamente, ma si è verificato un errore durante l'invio del messaggio di posta elettronica? Un'opzione potrebbe essere quella di generare un'eccezione e inviare l'utente alla pagina di errore. Questo potrebbe tuttavia confondere l'utente in modo che le informazioni immesse non siano state salvate. Un altro approccio consiste nel registrare l'errore relativo alla posta elettronica, ma non modificare in alcun modo l'esperienza dell'utente. Questa è la posizione in cui è utile la classe `ErrorSignal`.

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>Notifica di errore tramite posta elettronica

Oltre a registrare gli errori in un database, ELMAH può anche essere configurato in modo da inviare messaggi di posta elettronica a un destinatario specifico. Questa funzionalità viene fornita dal modulo `ErrorMailModule` HTTP; Pertanto, è necessario registrare questo modulo HTTP in `Web.config` per inviare i dettagli dell'errore tramite posta elettronica.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

Specificare quindi le informazioni sul messaggio di posta elettronica di errore nella sezione `<errorMail>` dell'elemento `<elmah>`, che indica il mittente e il destinatario del messaggio di posta elettronica, l'oggetto e se il messaggio di posta elettronica viene inviato in modo asincrono.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

Con le impostazioni sopra riportate, ogni volta che si verifica un errore di runtime ELMAH Invia un messaggio di posta elettronica a support@example.com con i dettagli dell'errore. Il messaggio di posta elettronica di errore di ELMAH include le stesse informazioni mostrate nella pagina web dettagli errore, ovvero il messaggio di errore, l'analisi dello stack e le variabili server (fare riferimento alle **figure 4** e **5**). Il messaggio di posta elettronica di errore include anche lo schermo giallo Dettagli eccezione del contenuto della morte come allegato (`YSOD.html`).

La **Figura 8** Mostra la posta elettronica di errore di ELMAH generata visitando `Genre.aspx?ID=foo`. Mentre la **Figura 8** Mostra solo il messaggio di errore e la traccia dello stack, le variabili server sono incluse più in basso nel corpo del messaggio di posta elettronica.

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**Figura 8**: è possibile configurare ELMAH per inviare i dettagli dell'errore tramite posta elettronica  
([Fare clic per visualizzare l'immagine con dimensioni complete](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Registrazione degli errori di interesse

Per impostazione predefinita, ELMAH registra i dettagli di ogni eccezione non gestita, inclusi 404 e altri errori HTTP. È possibile indicare a ELMAH di ignorare questi o altri tipi di errori usando il filtro degli errori. La logica di filtro viene eseguita dal modulo `ErrorFilterModule` HTTP di ELMAH, che è necessario registrare in `Web.config` per usare la logica di filtro. Le regole per l'applicazione di filtri sono specificate nella sezione `<errorFilter>`.

Il markup seguente indica a ELMAH di non registrare 404 errori.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> Per usare il filtro degli errori, è necessario registrare il modulo HTTP `ErrorFilterModule`.

L'elemento `<equal>` all'interno della sezione `<test>` viene definito asserzione. Se l'asserzione restituisce true, l'errore viene filtrato dal log di ELMAH. Sono disponibili altre asserzioni, tra cui: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`e così via. È inoltre possibile combinare asserzioni utilizzando gli operatori `<and>` e `<or>` booleani. Inoltre, è anche possibile includere un'espressione JavaScript semplice come asserzione oppure scrivere asserzioni personalizzate in C# o Visual Basic.

Per altre informazioni sulle funzionalità di filtro degli errori di ELMAH, vedere la [sezione filtro degli errori](https://code.google.com/p/elmah/wiki/ErrorFiltering) nel [wiki di ELMAH](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Riepilogo

ELMAH fornisce un meccanismo semplice ma potente per la registrazione degli errori in un'applicazione Web ASP.NET. Come il sistema di monitoraggio dello stato di Microsoft, ELMAH può registrare gli errori in un database e può inviare i dettagli dell'errore a uno sviluppatore tramite posta elettronica. A differenza del sistema di monitoraggio dello stato, ELMAH include il supporto predefinito per una gamma più ampia di archivi dati di log degli errori, tra cui: Microsoft SQL Server, Microsoft Access, Oracle, file XML e molti altri. Inoltre, ELMAH offre un meccanismo incorporato per la visualizzazione del log degli errori e i dettagli relativi a un errore specifico da una pagina Web `elmah.axd`. La pagina `elmah.axd` può inoltre eseguire il rendering delle informazioni sugli errori come feed RSS o come file con valori delimitati da virgole (CSV), che è possibile leggere utilizzando Microsoft Excel. È inoltre possibile indicare a ELMAH di filtrare gli errori dal log utilizzando asserzioni dichiarative o a livello di codice. E ELMAH possono essere usati con le applicazioni ASP.NET versione 1. x.

Ogni applicazione distribuita deve disporre di un meccanismo per registrare automaticamente le eccezioni non gestite e inviare notifiche al team di sviluppo. Se questa operazione viene eseguita utilizzando il monitoraggio dello stato di integrità o ELMAH è secondario. In altre parole, non importa molto se si usa il monitoraggio dello stato di integrità o ELMAH; valutare entrambi i sistemi e scegliere quello più adatto alle proprie esigenze. Ciò che è fondamentalmente importante è che è necessario mettere in atto un meccanismo per registrare le eccezioni non gestite nell'ambiente di produzione.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [ELMAH-errori di registrazione di moduli e gestori](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Pagina del progetto ELMAH](https://code.google.com/p/elmah/) (codice sorgente, esempi, wiki)
- [Collegamento di ELMAH in un'applicazione Web per intercettare le eccezioni non gestite](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (video)
- [Pagine del log degli errori di sicurezza](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Uso di moduli e gestori HTTP per creare componenti ASP.NET collegabili](https://msdn.microsoft.com/library/aa479332.aspx)
- [Esercitazioni sulla sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Precedente](logging-error-details-with-asp-net-health-monitoring-cs.md)
> [Successivo](precompiling-your-website-cs.md)
