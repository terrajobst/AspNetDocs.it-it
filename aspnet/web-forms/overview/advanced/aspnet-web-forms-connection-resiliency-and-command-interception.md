---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: Resilienza della connessione e intercettazione di comandi di ASP.NET Web Forms | Microsoft Docs
author: Erikre
description: Questa esercitazione illustra come modificare un'applicazione di esempio per supportare la resilienza della connessione e l'intercettazione di comandi.
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 95f0b5635c12d5ef88622e5766c1278c6570dd4d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598427"
---
# <a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Resilienza della connessione e intercettazione dei comandi di Web Forms ASP.NET

di [Erik Reitan](https://github.com/Erikre)

In questa esercitazione verrà modificata l'applicazione di esempio Wingtip Toys per supportare la resilienza della connessione e l'intercettazione dei comandi. Abilitando la resilienza della connessione, l'applicazione di esempio Wingtip Toys tenterà automaticamente di ripetere le chiamate ai dati quando si verificano errori temporanei tipici di un ambiente cloud. Inoltre, implementando l'intercettazione dei comandi, l'applicazione di esempio Wingtip Toys rileverà tutte le query SQL inviate al database per poterle registrare o modificare.

> [!NOTE] 
> 
> Questa esercitazione su Web Form è basata sull'esercitazione MVC seguente di Tom Dykstra:  
> [Resilienza della connessione e intercettazione dei comandi con il Entity Framework in un'applicazione MVC ASP.NET](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="what-youll-learn"></a>Contenuto dell'esercitazione:

- Come garantire la resilienza della connessione.
- Come implementare l'intercettazione di comandi.

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, verificare che nel computer sia installato il software seguente:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 per il Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Il .NET Framework viene installato automaticamente.
- Il progetto di esempio Wingtip Toys, in modo che sia possibile implementare la funzionalità descritta in questa esercitazione nel progetto Wingtip Toys. Il collegamento seguente fornisce i dettagli sul download:

    - [Introduzione con ASP.NET 4.5.1 Web Forms-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Prima di completare questa esercitazione, prendere in considerazione la revisione della serie di esercitazioni correlate, [Introduzione con Web Form e Visual Studio 2013 ASP.NET 4,5](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). La serie di esercitazioni consentirà di acquisire familiarità con il progetto e il codice **WingtipToys** .

## <a name="connection-resiliency"></a>Resilienza della connessione

Quando si considera la distribuzione di un'applicazione in Windows Azure, un'opzione da considerare è la distribuzione del database nel **database SQL**di **Windows** Azure, un servizio di database cloud. Gli errori di connessione temporanei sono in genere più frequenti quando ci si connette a un servizio di database cloud rispetto a quando il server Web e il server di database sono connessi direttamente nello stesso data center. Anche se un server Web Cloud e un servizio di database cloud sono ospitati nello stesso data center, esistono più connessioni di rete tra di esse che possono avere problemi, ad esempio i servizi di bilanciamento del carico.

Inoltre, un servizio cloud viene in genere condiviso da altri utenti, il che significa che la velocità di risposta può essere interessata da tali utenti. E l'accesso al database potrebbe essere soggetto alla limitazione delle richieste. La limitazione indica che il servizio di database genera eccezioni quando si tenta di accedervi con una frequenza maggiore di quella consentita nel *contratto di servizio* (SLA).

Molti o più problemi di connessione che si verificano quando si accede a un servizio cloud sono temporanei, ovvero si risolvono in un breve periodo di tempo. Pertanto, quando si tenta di eseguire un'operazione sul database e si ottiene un tipo di errore in genere temporaneo, è possibile riprovare l'operazione dopo una breve attesa e l'operazione potrebbe avere esito positivo. È possibile offrire agli utenti un'esperienza molto migliore se si gestiscono errori temporanei ritentando automaticamente la maggior parte dei casi, rendendola invisibile al cliente. La funzionalità di resilienza della connessione in Entity Framework 6 consente di automatizzare il processo di ripetizione delle query SQL non riuscite.

La funzionalità di resilienza della connessione deve essere configurata in modo appropriato per un particolare servizio di database:

1. È necessario stabilire quali eccezioni sono probabilmente temporanee. Si desidera ritentare gli errori causati da una perdita temporanea della connettività di rete, ad esempio errori causati da bug del programma.
2. Deve attendere un periodo di tempo appropriato tra i tentativi di un'operazione non riuscita. È possibile attendere più a lungo tra i tentativi per un processo batch rispetto a una pagina Web online in cui un utente è in attesa di una risposta.
3. Deve ritentare un numero di volte appropriato prima di rinunciare. Potrebbe essere necessario ritentare più volte in un processo batch in un'applicazione online.

È possibile configurare queste impostazioni manualmente per qualsiasi ambiente di database supportato da un provider Entity Framework.

Per abilitare la resilienza della connessione è sufficiente creare una classe nell'assembly che deriva dalla classe `DbConfiguration` e in tale classe impostare la strategia di esecuzione del database SQL, che in Entity Framework è un altro termine per i criteri di ripetizione dei tentativi.

### <a name="implementing-connection-resiliency"></a>Implementazione della resilienza delle connessioni

1. Scaricare e aprire l'applicazione Web Forms di esempio [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) in Visual Studio.
2. Nella cartella della *logica* dell'applicazione **WingtipToys** aggiungere un file di classe denominato *WingtipToysConfiguration.cs*.
3. Sostituire il codice esistente con il seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Il Entity Framework esegue automaticamente il codice trovato in una classe che deriva da `DbConfiguration`. È possibile utilizzare la classe `DbConfiguration` per eseguire attività di configurazione nel codice che altrimenti verrebbe eseguita nel file *Web. config* . Per altre informazioni, vedere [configurazione basata su codice EntityFramework](https://msdn.microsoft.com/data/jj680699).

1. Nella cartella *logica* aprire il file *AddProducts.cs* .
2. Aggiungere un'istruzione `using` per `System.Data.Entity.Infrastructure`, come illustrato in giallo:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Aggiungere un blocco `catch` al metodo `AddProduct` in modo che il `RetryLimitExceededException` venga registrato come evidenziato in giallo:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Aggiungendo l'eccezione di `RetryLimitExceededException`, è possibile fornire una registrazione migliore o visualizzare un messaggio di errore all'utente in cui è possibile scegliere di riprovare il processo. Intercettando l'eccezione di `RetryLimitExceededException`, gli unici errori che probabilmente sarebbero temporanei saranno già stati tentati e non riusciranno più volte. L'eccezione effettiva restituita verrà racchiusa nell'eccezione `RetryLimitExceededException`. Inoltre, è stato aggiunto un blocco catch generale. Per ulteriori informazioni sull'eccezione di `RetryLimitExceededException`, vedere [Entity Framework la resilienza della connessione e la logica di ripetizione dei tentativi](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Intercettazione comandi

Ora che è stato attivato un criterio di ripetizione dei tentativi, come si esegue il test per verificare che funzioni come previsto? Non è così facile forzare l'esecuzione di un errore temporaneo, soprattutto quando si esegue localmente, ed è particolarmente difficile integrare gli errori temporanei effettivi in un unit test automatizzato. Per testare la funzionalità di resilienza della connessione, è necessario un modo per intercettare le query che Entity Framework invia a SQL Server e sostituire la risposta SQL Server con un tipo di eccezione che in genere è temporaneo.

È anche possibile usare l'intercettazione di query per implementare una procedura consigliata per le applicazioni cloud: registrare la latenza e l'esito positivo o negativo di tutte le chiamate a servizi esterni, ad esempio i servizi di database.

In questa sezione dell'esercitazione si utilizzerà la [*funzionalità di intercettazione*](https://msdn.microsoft.com/data/dn469464) del Entity Framework per la registrazione e la simulazione degli errori temporanei.

### <a name="create-a-logging-interface-and-class"></a>Creare un'interfaccia di registrazione e una classe

Una procedura consigliata per la registrazione consiste nell'eseguire questa operazione utilizzando un [`interface`](https://msdn.microsoft.com/library/ms173156.aspx) piuttosto che le chiamate a hardcoded per `System.Diagnostics.Trace` o una classe di registrazione. Questo rende più semplice modificare il meccanismo di registrazione in un secondo momento, se necessario. In questa sezione verrà creata l'interfaccia di registrazione e una classe per implementarla.

In base alla procedura sopra riportata, è stata scaricata e aperta l'applicazione di esempio **WingtipToys** in Visual Studio.

1. Creare una cartella nel progetto **WingtipToys** e denominarla *Logging*.
2. Nella cartella *registrazione* creare un file di classe denominato *ILogger.cs* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   L'interfaccia fornisce tre livelli di traccia per indicare l'importanza relativa dei log e una progettata per fornire informazioni sulla latenza per le chiamate al servizio esterno, ad esempio le query del database. I metodi di registrazione hanno overload che consentono di passare un'eccezione. In questo modo, le informazioni sulle eccezioni incluse la traccia dello stack e le eccezioni interne vengono registrate in modo affidabile dalla classe che implementa l'interfaccia, invece di basarsi su tale operazione in ogni chiamata al metodo di registrazione nell'intera applicazione.  
  
   I metodi `TraceApi` consentono di tenere traccia della latenza di ogni chiamata a un servizio esterno, ad esempio il database SQL.
3. Nella cartella *registrazione* creare un file di classe denominato *logger.cs* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

L'implementazione utilizza `System.Diagnostics` per eseguire la traccia. Si tratta di una funzionalità incorporata di .NET che consente di generare e utilizzare facilmente le informazioni di traccia. Ci sono molti listener di &quot;&quot; è possibile usare con la traccia `System.Diagnostics`, per scrivere log in file, ad esempio, o per scriverli nell'archiviazione BLOB in Azure. Per ulteriori informazioni, vedere la sezione relativa alla [risoluzione dei problemi relativi ai siti Web di Microsoft Azure in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Per questa esercitazione verranno esaminati solo i log nella finestra di **output** di Visual Studio.

In un'applicazione di produzione può essere opportuno prendere in considerazione l'uso di Framework di traccia diversi da `System.Diagnostics`e l'interfaccia `ILogger` rende relativamente semplice passare a un meccanismo di traccia diverso se si decide di eseguire questa operazione.

### <a name="create-interceptor-classes"></a>Creare classi intercettore

Successivamente, verranno create le classi che il Entity Framework chiamerà in ogni volta che invierà una query al database, una per simulare gli errori temporanei e l'altra per eseguire la registrazione. Queste classi dell'intercettore devono derivare dalla classe `DbCommandInterceptor`. In essi si scrivono override del metodo che vengono chiamati automaticamente quando la query sta per essere eseguita. In questi metodi è possibile esaminare o registrare la query che viene inviata al database ed è possibile modificare la query prima di inviarla al database o restituire un elemento Entity Framework senza nemmeno passare la query al database.

1. Per creare la classe interceptor che registrerà ogni query SQL prima che venga inviata al database, creare un file di classe denominato *InterceptorLogging.cs* nella cartella della *logica* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Per le query o i comandi riusciti, questo codice scrive un log informazioni con le informazioni sulla latenza. Per le eccezioni, viene creato un log degli errori.
2. Per creare la classe interceptor che genererà errori temporanei fittizi quando si immette &quot;generare&quot; nella casella di testo **nome** nella pagina denominata *AdminPage. aspx*, creare un file di classe denominato *InterceptorTransientErrors.cs* nella cartella della *logica* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Questo codice esegue l'override solo del metodo `ReaderExecuting`, che viene chiamato per le query che possono restituire più righe di dati. Se si desidera controllare la resilienza della connessione per altri tipi di query, è anche possibile eseguire l'override dei metodi `NonQueryExecuting` e `ScalarExecuting`, come avviene per l'intercettore di registrazione.  
  
   Successivamente, si accederà come "amministratore" e si seleziona il collegamento **amministratore** sulla barra di spostamento superiore. Quindi, nella pagina *AdminPage. aspx* verrà aggiunto un prodotto denominato &quot;Throw&quot;. Il codice crea un'eccezione fittizia del database SQL per il numero di errore 20, un tipo noto in genere come temporaneo. Altri numeri di errore attualmente riconosciuti come temporanei sono 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, ma sono soggetti a modifiche nelle nuove versioni del database SQL. Il prodotto verrà rinominato in "TransientErrorExample", che è possibile seguire nel codice del file *InterceptorTransientErrors.cs* .  
  
   Il codice restituisce l'eccezione a Entity Framework anziché eseguire la query e restituire i risultati. L'eccezione temporanea viene restituita *quattro* volte, quindi il codice ripristina la normale procedura di passaggio della query al database.

    Poiché tutto viene registrato, sarà possibile vedere che Entity Framework tenta di eseguire la query quattro volte prima di avere esito positivo e l'unica differenza nell'applicazione è che richiede più tempo per eseguire il rendering di una pagina con i risultati della query.  
  
   Numero di tentativi configurabili per il Entity Framework. il codice specifica quattro volte perché questo è il valore predefinito per i criteri di esecuzione del database SQL. Se si modificano i criteri di esecuzione, è necessario modificare anche il codice che specifica il numero di volte in cui vengono generati errori temporanei. È anche possibile modificare il codice per generare più eccezioni in modo che Entity Framework genererà l'eccezione `RetryLimitExceededException`.
3. In *Global. asax*aggiungere le istruzioni using seguenti:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Aggiungere quindi le righe evidenziate al metodo `Application_Start`:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Queste righe di codice sono la causa dell'esecuzione del codice dell'intercettore quando Entity Framework invia query al database. Si noti che poiché sono state create classi di intercettori separate per la simulazione e la registrazione degli errori temporanei, è possibile abilitarle e disabilitarle in modo indipendente.   
  
 È possibile aggiungere intercettori usando il metodo `DbInterception.Add` in qualsiasi punto del codice. non deve trovarsi nel metodo `Application_Start`. Un'altra opzione, se non sono stati aggiunti intercettori nel metodo `Application_Start`, potrebbe aggiornare o aggiungere la classe denominata *WingtipToysConfiguration.cs* e inserire il codice precedente alla fine del costruttore della classe `WingtipToysConfiguration`.

Quando si inserisce questo codice, prestare attenzione a non eseguire `DbInterception.Add` per lo stesso intercettore più di una volta o si otterranno istanze di intercettori aggiuntive. Ad esempio, se si aggiunge l'intercettore di registrazione due volte, verranno visualizzati due log per ogni query SQL.

Gli intercettori vengono eseguiti nell'ordine di registrazione, ovvero l'ordine in cui viene chiamato il metodo `DbInterception.Add`. L'ordine potrebbe essere importante a seconda di ciò che si sta eseguendo nell'intercettore. Ad esempio, un intercettore potrebbe modificare il comando SQL che ottiene nella proprietà `CommandText`. Se viene modificato il comando SQL, l'intercettore successivo otterrà il comando SQL modificato e non il comando SQL originale.

Il codice di simulazione degli errori temporanei è stato scritto in modo che consenta di causare errori temporanei immettendo un valore diverso nell'interfaccia utente. In alternativa, è possibile scrivere il codice dell'intercettore per generare sempre la sequenza di eccezioni temporanee senza verificare la presenza di un particolare valore di parametro. È quindi possibile aggiungere l'intercettore solo quando si desidera generare errori temporanei. Se si esegue questa operazione, tuttavia, non aggiungere l'intercettore finché non viene completata l'inizializzazione del database. In altre parole, eseguire almeno un'operazione di database, ad esempio una query su uno dei set di entità prima di iniziare a generare errori temporanei. Il Entity Framework esegue diverse query durante l'inizializzazione del database e non vengono eseguite in una transazione, quindi gli errori durante l'inizializzazione potrebbero causare uno stato incoerente del contesto.

## <a name="test-logging-and-connection-resiliency"></a>Registrazione test e resilienza della connessione

1. In Visual Studio premere **F5** per eseguire l'applicazione in modalità di debug, quindi accedere come "admin" utilizzando "Pa $ $Word" come password.
2. Selezionare **amministratore** nella barra di spostamento in alto.
3. Immettere un nuovo prodotto denominato "Throw" con la descrizione, il prezzo e il file di immagine appropriati.
4. Premere il pulsante **Aggiungi prodotto** .  
   Si noterà che il browser sembra bloccarsi per alcuni secondi, mentre Entity Framework ritenta la query più volte. Il primo tentativo viene eseguito molto rapidamente, quindi l'attesa aumenta prima di ogni ulteriore tentativo. Questo processo di attesa più a lungo prima di ogni nuovo tentativo viene chiamato *backoff esponenziale* .
5. Attendere fino a quando non viene più eseguito un tentativo di caricamento della pagina.
6. Arrestare il progetto ed esaminare la finestra di **output** di Visual Studio per visualizzare l'output di traccia. È possibile trovare la finestra di **output** selezionando **Debug** -&gt; -**Windows** &gt; **output**. Potrebbe essere necessario scorrere oltre molti altri log scritti dal logger.  
  
   Si noti che è possibile visualizzare le query SQL effettive inviate al database. Vengono visualizzati alcuni comandi e query iniziali che Entity Framework per iniziare, verificando la versione del database e la tabella di cronologia della migrazione.   
    ![Finestra di output](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Si noti che non è possibile ripetere questo test a meno che l'applicazione non venga arrestata e riavviata. Per poter testare più volte la resilienza delle connessioni in una singola esecuzione dell'applicazione, è possibile scrivere codice per reimpostare il contatore degli errori in `InterceptorTransientErrors`.
7. Per visualizzare la differenza tra la strategia di esecuzione (criteri di ripetizione dei tentativi), impostare come commento la riga `SetExecutionStrategy` nel file *WingtipToysConfiguration.cs* nella cartella della *logica* , eseguire di nuovo la pagina di **Amministrazione** in modalità di debug e aggiungere il prodotto denominato &quot;generare nuovamente&quot;.  
  
   Questa volta il debugger si arresta sulla prima eccezione generata immediatamente quando tenta di eseguire la query la prima volta.  
    ![Debug-Visualizza dettagli](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Rimuovere il commento dalla riga `SetExecutionStrategy` nel file *WingtipToysConfiguration.cs* .

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come modificare un'applicazione di esempio Web Form per supportare la resilienza della connessione e l'intercettazione di comandi.

## <a name="next-steps"></a>Passaggi successivi

Dopo aver esaminato la resilienza della connessione e l'intercettazione dei comandi in ASP.NET Web Form, vedere l'argomento ASP.NET Web Forms [asincrono Methods in ASP.NET 4,5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). In questo argomento vengono illustrate le nozioni di base per la creazione di un'applicazione Web Form ASP.NET asincrona mediante Visual Studio.
