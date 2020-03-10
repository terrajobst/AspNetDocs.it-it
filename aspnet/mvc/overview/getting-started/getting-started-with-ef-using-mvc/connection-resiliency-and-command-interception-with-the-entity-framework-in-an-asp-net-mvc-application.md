---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: usare la resilienza delle connessioni e l'intercettazione dei comandi con EF in un'app MVC ASP.NET"
author: tdykstra
description: Questa esercitazione illustra come usare la resilienza delle connessioni e l'intercettazione di comandi. Sono due importanti funzionalità di Entity Framework 6.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 276266f8ae9df38529d44742ebe6ac0dc8e79727
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583454"
---
# <a name="tutorial-use-connection-resiliency-and-command-interception-with-entity-framework-in-an-aspnet-mvc-app"></a>Esercitazione: usare la resilienza delle connessioni e l'intercettazione dei comandi con Entity Framework in un'app MVC ASP.NET

Fino a questo punto l'applicazione è stata eseguita localmente in IIS Express nel computer di sviluppo. Per rendere disponibile un'applicazione reale per altre persone da usare su Internet, è necessario distribuirla a un provider di hosting Web ed è necessario distribuire il database in un server di database.

Questa esercitazione illustra come usare la resilienza delle connessioni e l'intercettazione di comandi. Sono due importanti funzionalità di Entity Framework 6 che risultano particolarmente utili quando si esegue la distribuzione nell'ambiente cloud: resilienza della connessione (tentativi automatici per gli errori temporanei) e intercettazione dei comandi (intercettare tutte le query SQL inviate al database per registrarli o modificarli.

Questa esercitazione sulla resilienza della connessione e sull'intercettazione comandi è facoltativa. Se si ignora questa esercitazione, sarà necessario apportare alcune modifiche secondarie nelle esercitazioni successive.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Abilita resilienza connessione
> * Abilita intercettazione comandi
> * Testare la nuova configurazione

## <a name="prerequisites"></a>Prerequisiti

* [Ordinamento, filtro e paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-connection-resiliency"></a>Abilita resilienza connessione

Quando si distribuisce l'applicazione in Windows Azure, il database verrà distribuito nel database SQL di Windows Azure, un servizio di database cloud. Gli errori di connessione temporanei sono in genere più frequenti quando ci si connette a un servizio di database cloud rispetto a quando il server Web e il server di database sono connessi direttamente nello stesso data center. Anche se un server Web Cloud e un servizio di database cloud sono ospitati nello stesso data center, esistono più connessioni di rete tra di esse che possono avere problemi, ad esempio i servizi di bilanciamento del carico.

Inoltre, un servizio cloud viene in genere condiviso da altri utenti, il che significa che la velocità di risposta può essere interessata da tali utenti. E l'accesso al database potrebbe essere soggetto alla limitazione delle richieste. La limitazione indica che il servizio di database genera eccezioni quando si tenta di accedervi con una frequenza maggiore di quella consentita nel Contratto di servizio (SLA).

Molti o più problemi di connessione quando si accede a un servizio cloud sono temporanei, ovvero si risolvono in un breve periodo di tempo. Pertanto, quando si tenta di eseguire un'operazione sul database e si ottiene un tipo di errore in genere temporaneo, è possibile riprovare l'operazione dopo una breve attesa e l'operazione potrebbe avere esito positivo. È possibile offrire agli utenti un'esperienza molto migliore se si gestiscono errori temporanei ritentando automaticamente la maggior parte dei casi, rendendola invisibile al cliente. La funzionalità di resilienza della connessione in Entity Framework 6 consente di automatizzare il processo di ripetizione delle query SQL non riuscite.

La funzionalità di resilienza della connessione deve essere configurata in modo appropriato per un particolare servizio di database:

- È necessario stabilire quali eccezioni sono probabilmente temporanee. Si desidera ritentare gli errori causati da una perdita temporanea della connettività di rete, ad esempio errori causati da bug del programma.
- Deve attendere un periodo di tempo appropriato tra i tentativi di un'operazione non riuscita. È possibile attendere più a lungo tra i tentativi per un processo batch rispetto a una pagina Web online in cui un utente è in attesa di una risposta.
- Deve ritentare un numero di volte appropriato prima di rinunciare. Potrebbe essere necessario ritentare più volte in un processo batch in un'applicazione online.

È possibile configurare queste impostazioni manualmente per qualsiasi ambiente di database supportato da un provider Entity Framework, ma i valori predefiniti che in genere funzionano correttamente per un'applicazione online che utilizza il database SQL di Windows Azure sono già stati configurati e Queste sono le impostazioni che verranno implementate per l'applicazione Contoso University.

Per abilitare la resilienza della connessione è sufficiente creare una classe nell'assembly che deriva dalla classe [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) e in tale classe impostare la *strategia di esecuzione*del database SQL, che in EF è un altro termine per i criteri di *ripetizione dei tentativi*.

1. Nella cartella DAL aggiungere un file di classe denominato *SchoolConfiguration.cs*.
2. Sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Il Entity Framework esegue automaticamente il codice trovato in una classe che deriva da `DbConfiguration`. È possibile utilizzare la classe `DbConfiguration` per eseguire attività di configurazione nel codice che altrimenti verrebbe eseguita nel file *Web. config* . Per altre informazioni, vedere [configurazione basata su codice EntityFramework](https://msdn.microsoft.com/data/jj680699).
3. In *StudentController.cs*aggiungere un'istruzione `using` per `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Modificare tutti i blocchi di `catch` che intercettano `DataException` eccezioni in modo da intercettare `RetryLimitExceededException` eccezioni. Esempio:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Si stava usando `DataException` per tentare di identificare gli errori che potrebbero essere temporanei per fornire un messaggio di "Riprova". Tuttavia, ora che è stato attivato un criterio di ripetizione, gli unici errori che probabilmente sarebbero temporanei saranno già stati tentati e non saranno riusciti più volte e l'eccezione effettiva restituita verrà racchiusa nell'eccezione `RetryLimitExceededException`.

Per altre informazioni, vedere [Entity Framework la resilienza della connessione/logica di ripetizione dei tentativi](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Abilita intercettazione comandi

Ora che è stato attivato un criterio di ripetizione dei tentativi, come si esegue il test per verificare che funzioni come previsto? Non è così facile forzare l'esecuzione di un errore temporaneo, soprattutto quando si esegue localmente, ed è particolarmente difficile integrare gli errori temporanei effettivi in un unit test automatizzato. Per testare la funzionalità di resilienza della connessione, è necessario un modo per intercettare le query che Entity Framework invia a SQL Server e sostituire la risposta SQL Server con un tipo di eccezione che in genere è temporaneo.

È anche possibile usare l'intercettazione di query per implementare una procedura consigliata per le applicazioni cloud: [registrare la latenza e l'esito positivo o negativo di tutte le chiamate a servizi esterni](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) , ad esempio i servizi di database. EF6 fornisce un' [API di registrazione dedicata](https://msdn.microsoft.com/data/dn469464) che può semplificare la registrazione, ma in questa sezione dell'esercitazione si apprenderà come usare la [funzionalità di intercettazione](https://msdn.microsoft.com/data/dn469464) del Entity Framework direttamente, sia per la registrazione che per la simulazione degli errori temporanei.

### <a name="create-a-logging-interface-and-class"></a>Creare un'interfaccia di registrazione e una classe

Una [procedura consigliata per la registrazione](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) consiste nell'eseguire questa operazione usando un'interfaccia anziché le chiamate hardcoded a System. Diagnostics. Trace o a una classe di registrazione. Questo rende più semplice modificare il meccanismo di registrazione in un secondo momento, se necessario. Quindi, in questa sezione verrà creata l'interfaccia di registrazione e una classe per implementarla./p >

1. Creare una cartella nel progetto e denominarla *registrazione*.
2. Nella cartella *registrazione* creare un file di classe denominato *ILogger.cs*e sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    L'interfaccia fornisce tre livelli di traccia per indicare l'importanza relativa dei log e una progettata per fornire informazioni sulla latenza per le chiamate al servizio esterno, ad esempio le query del database. I metodi di registrazione hanno overload che consentono di passare un'eccezione. In questo modo, le informazioni sulle eccezioni incluse la traccia dello stack e le eccezioni interne vengono registrate in modo affidabile dalla classe che implementa l'interfaccia, invece di basarsi su tale operazione in ogni chiamata al metodo di registrazione nell'intera applicazione.

    I metodi TraceApi consentono di tenere traccia della latenza di ogni chiamata a un servizio esterno, ad esempio il database SQL.
3. Nella cartella *registrazione* creare un file di classe denominato *logger.cs*e sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    L'implementazione di USA System. Diagnostics per eseguire la traccia. Si tratta di una funzionalità incorporata di .NET che consente di generare e utilizzare facilmente le informazioni di traccia. Ci sono molti "listener" che è possibile usare con la traccia System. Diagnostics per scrivere i log nei file, ad esempio, o per scriverli nell'archivio BLOB in Azure. Per ulteriori informazioni, vedere la sezione relativa alla [risoluzione dei problemi relativi ai siti Web di Azure in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Per questa esercitazione verranno esaminati solo i log nella finestra di **output** di Visual Studio.

    In un'applicazione di produzione può essere opportuno prendere in considerazione la traccia di pacchetti diversi da System. Diagnostics e l'interfaccia ILogger rende relativamente semplice passare a un meccanismo di traccia diverso se si decide di eseguire questa operazione.

### <a name="create-interceptor-classes"></a>Creare classi intercettore

Verranno quindi create le classi che verranno chiamate dal Entity Framework ogni volta che viene inviata una query al database, una per simulare gli errori temporanei e l'altra per eseguire la registrazione. Queste classi dell'intercettore devono derivare dalla classe `DbCommandInterceptor`. In questi casi si scrivono override dei metodi che vengono chiamati automaticamente quando la query sta per essere eseguita. In questi metodi è possibile esaminare o registrare la query che viene inviata al database ed è possibile modificare la query prima di inviarla al database o restituire un elemento Entity Framework senza nemmeno passare la query al database.

1. Per creare la classe interceptor che registrerà ogni query SQL inviata al database, creare un file di classe denominato *SchoolInterceptorLogging.cs* nella cartella *dal* e sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Per le query o i comandi riusciti, questo codice scrive un log informazioni con le informazioni sulla latenza. Per le eccezioni, viene creato un log degli errori.
2. Per creare la classe interceptor che genererà errori temporanei fittizi quando si immette "Throw" nella casella di **ricerca** , creare un file di classe denominato *SchoolInterceptorTransientErrors.cs* nella cartella *dal* e sostituire il codice del modello con il codice seguente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Questo codice esegue l'override solo del metodo `ReaderExecuting`, che viene chiamato per le query che possono restituire più righe di dati. Se si desidera controllare la resilienza della connessione per altri tipi di query, è anche possibile eseguire l'override dei metodi `NonQueryExecuting` e `ScalarExecuting`, come avviene per l'intercettore di registrazione.

    Quando si esegue la pagina Student e si immette "Throw" come stringa di ricerca, questo codice crea un'eccezione fittizia del database SQL per il numero di errore 20, un tipo noto come in genere temporaneo. Altri numeri di errore attualmente riconosciuti come temporanei sono 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, ma sono soggetti a modifiche nelle nuove versioni del database SQL.

    Il codice restituisce l'eccezione a Entity Framework anziché eseguire la query e passare i risultati della query. L'eccezione temporanea viene restituita quattro volte, quindi il codice ripristina la normale procedura di passaggio della query al database.

    Poiché tutto viene registrato, sarà possibile vedere che Entity Framework tenta di eseguire la query quattro volte prima di avere esito positivo e l'unica differenza nell'applicazione è che richiede più tempo per eseguire il rendering di una pagina con i risultati della query.

    Numero di tentativi configurabili per il Entity Framework. il codice specifica quattro volte perché questo è il valore predefinito per i criteri di esecuzione del database SQL. Se si modificano i criteri di esecuzione, è necessario modificare anche il codice che specifica il numero di volte in cui vengono generati errori temporanei. È anche possibile modificare il codice per generare più eccezioni in modo che Entity Framework genererà l'eccezione `RetryLimitExceededException`.

    Il valore immesso nella casella di ricerca sarà `command.Parameters[0]` e `command.Parameters[1]` (verrà usato uno per il nome e uno per il cognome). Quando viene trovato il valore "% Throw%", "Throw" viene sostituito in questi parametri da "an" in modo che alcuni studenti vengano trovati e restituiti.

    Questo è semplicemente un modo pratico per testare la resilienza della connessione in base alla modifica di un input nell'interfaccia utente dell'applicazione. È anche possibile scrivere codice che genera errori temporanei per tutte le query o gli aggiornamenti, come illustrato più avanti nei commenti sul metodo *DbInterception. Add* .
3. In *Global. asax*aggiungere le istruzioni `using` seguenti:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Aggiungere le righe evidenziate al metodo `Application_Start`:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Queste righe di codice sono la causa dell'esecuzione del codice dell'intercettore quando Entity Framework invia query al database. Si noti che poiché sono state create classi di intercettori separate per la simulazione e la registrazione degli errori temporanei, è possibile abilitarle e disabilitarle in modo indipendente.

    È possibile aggiungere intercettori usando il metodo `DbInterception.Add` in qualsiasi punto del codice. non deve trovarsi nel metodo `Application_Start`. Un'altra opzione consiste nell'inserire questo codice nella classe DbConfiguration creata in precedenza per configurare i criteri di esecuzione.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Quando si inserisce questo codice, prestare attenzione a non eseguire `DbInterception.Add` per lo stesso intercettore più di una volta o si otterranno istanze di intercettori aggiuntive. Ad esempio, se si aggiunge l'intercettore di registrazione due volte, verranno visualizzati due log per ogni query SQL.

    Gli intercettori vengono eseguiti nell'ordine di registrazione, ovvero l'ordine in cui viene chiamato il metodo `DbInterception.Add`. L'ordine potrebbe essere importante a seconda di ciò che si sta eseguendo nell'intercettore. Ad esempio, un intercettore potrebbe modificare il comando SQL che ottiene nella proprietà `CommandText`. Se viene modificato il comando SQL, l'intercettore successivo otterrà il comando SQL modificato e non il comando SQL originale.

    Il codice di simulazione degli errori temporanei è stato scritto in modo che consenta di causare errori temporanei immettendo un valore diverso nell'interfaccia utente. In alternativa, è possibile scrivere il codice dell'intercettore per generare sempre la sequenza di eccezioni temporanee senza verificare la presenza di un particolare valore di parametro. È quindi possibile aggiungere l'intercettore solo quando si desidera generare errori temporanei. Se si esegue questa operazione, tuttavia, non aggiungere l'intercettore finché non viene completata l'inizializzazione del database. In altre parole, eseguire almeno un'operazione di database, ad esempio una query su uno dei set di entità prima di iniziare a generare errori temporanei. Il Entity Framework esegue diverse query durante l'inizializzazione del database e non vengono eseguite in una transazione, quindi gli errori durante l'inizializzazione potrebbero causare uno stato incoerente del contesto.

## <a name="test-the-new-configuration"></a>Testare la nuova configurazione

1. Premere **F5** per eseguire l'applicazione in modalità di debug, quindi fare clic sulla scheda **students (studenti** ).
2. Esaminare la finestra di **output** di Visual Studio per visualizzare l'output di traccia. Potrebbe essere necessario scorrere oltre alcuni errori JavaScript per ottenere i log scritti dal logger.

    Si noti che è possibile visualizzare le query SQL effettive inviate al database. Vengono visualizzati alcuni comandi e query iniziali che Entity Framework per iniziare, verificando la versione del database e la tabella di cronologia della migrazione (si apprenderanno le migrazioni nell'esercitazione successiva). Viene visualizzata una query per il paging, per scoprire quanti studenti sono disponibili e infine viene visualizzata la query che ottiene i dati degli studenti.

    ![Registrazione per query normali](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Nella pagina **students (studenti** ) immettere "Throw" come stringa di ricerca e fare clic su **Search (Cerca**).

    ![Genera stringa di ricerca](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Si noterà che il browser sembra bloccarsi per alcuni secondi, mentre Entity Framework ritenta la query più volte. Il primo tentativo viene eseguito molto rapidamente, quindi l'attesa prima dell'aumento prima di ogni ulteriore tentativo. Questo processo di attesa più a lungo prima di ogni nuovo tentativo viene chiamato *backoff esponenziale*.

    Quando viene visualizzata la pagina, che Mostra gli studenti con "an" nei nomi, osservare la finestra di output e si noterà che la stessa query è stata tentata cinque volte, le prime quattro volte che restituiscono eccezioni temporanee. Per ogni errore temporaneo verrà visualizzato il log scritto durante la generazione dell'errore temporaneo nella classe `SchoolInterceptorTransientErrors` ("restituzione di un errore temporaneo per il comando...") e il log verrà scritto quando `SchoolInterceptorLogging` otterrà l'eccezione.

    ![Output di registrazione che mostra i tentativi](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Poiché è stata immessa una stringa di ricerca, la query che restituisce i dati degli studenti è parametrizzata:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Non si sta registrando il valore dei parametri, ma è possibile farlo. Se si desidera visualizzare i valori dei parametri, è possibile scrivere il codice di registrazione per ottenere i valori dei parametri dalla proprietà `Parameters` dell'oggetto `DbCommand` ottenuto nei metodi dell'intercettore.

    Si noti che non è possibile ripetere questo test a meno che l'applicazione non venga arrestata e riavviata. Per poter testare più volte la resilienza delle connessioni in una singola esecuzione dell'applicazione, è possibile scrivere codice per reimpostare il contatore degli errori in `SchoolInterceptorTransientErrors`.
4. Per visualizzare la differenza tra la strategia di esecuzione (criteri di ripetizione dei tentativi), impostare come commento la riga `SetExecutionStrategy` in *SchoolConfiguration.cs*, eseguire di nuovo la pagina students in modalità di debug e cercare di nuovo "Throw".

    Questa volta il debugger si arresta sulla prima eccezione generata immediatamente quando tenta di eseguire la query la prima volta.

    ![Eccezione fittizia](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Rimuovere il commento dalla riga *SetExecutionStrategy* in *SchoolConfiguration.cs*.

## <a name="get-the-code"></a>Ottenere il codice

[Scarica progetto completato](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Risorse aggiuntive

Collegamenti ad altre risorse Entity Framework sono disponibili in [ASP.NET Data Access-risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Resilienza della connessione abilitata
> * Intercettazione comandi abilitata
> * Test della nuova configurazione

Passare all'articolo successivo per informazioni sulle migrazioni Code First e sulla distribuzione di Azure.
> [!div class="nextstepaction"]
> [Migrazioni di Code First e distribuzione di Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
