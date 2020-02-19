---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: "Appendice: l'applicazione di esempio Fix it (compilazione di app Cloud reali con Azure) | Microsoft Docs"
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 896196bdb6a6b0d12a6c798ead510e37dd38a9fc
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456881"
---
# <a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Appendice: l'applicazione di esempio Fix it (compilazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Correggi it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Questa appendice per la creazione di app Cloud reali con e-book di Azure contiene le sezioni seguenti che forniscono informazioni aggiuntive sull'applicazione di esempio Fix it che è possibile scaricare:

- [Problemi noti](#knownissues)
- [Procedure consigliate](#bestpractices)
- [Come eseguire l'app da Visual Studio nel computer locale](#run-in-vs)
- [Come distribuire l'app di base in app Web del servizio app Azure usando gli script di Windows PowerShell](#deploybase)
- [Risoluzione dei problemi relativi agli script di Windows PowerShell](#troubleshooting)
- [Come distribuire l'app con l'elaborazione delle code in app Web del servizio app Azure e un servizio cloud di Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemi noti

L'app per la correzione è stata originariamente sviluppata per illustrare il più semplice possibile alcuni dei modelli presentati in questo e-book. Tuttavia, poiché l'e-book riguarda la creazione di app reali, abbiamo sottoposto il codice di correzione a un processo di revisione e test simile a quello che avremmo fatto per il software rilasciato. Sono stati rilevati diversi problemi e, come per qualsiasi applicazione reale, alcuni di essi sono stati corretti e alcuni di essi sono stati rilasciati a una versione successiva.

Nell'elenco seguente sono inclusi i problemi che devono essere risolti in un'applicazione di produzione, ma per un motivo o un altro si è deciso di non risolvere nella versione iniziale dell'applicazione di esempio Fix it.

### <a name="security"></a>Sicurezza

- Assicurarsi che non sia possibile assegnare un'attività a un proprietario inesistente.
- Assicurarsi che sia possibile visualizzare e modificare solo le attività create o assegnate all'utente.
- Usare HTTPS per le pagine di accesso e i cookie di autenticazione.
- Specificare un limite di tempo per i cookie di autenticazione.

### <a name="input-validation"></a>Convalida dell'input

In generale, un'app di produzione esegue una maggiore convalida dell'input rispetto all'app per la correzione. Ad esempio, la dimensione dell'immagine o le dimensioni del file di immagine consentite per il caricamento devono essere limitate.

### <a name="administrator-functionality"></a>Funzionalità amministratore

Un amministratore deve essere in grado di modificare la proprietà per le attività esistenti. Il creatore di un'attività, ad esempio, potrebbe lasciare l'azienda, lasciando che non sia disponibile l'autorizzazione a mantenere l'attività a meno che non sia abilitato l'accesso amministrativo.

### <a name="queue-message-processing"></a>Elaborazione messaggi in coda

L'elaborazione dei messaggi in coda nell'app per la correzione è stata progettata per essere semplice per illustrare il modello di lavoro incentrato sulle code con una quantità minima di codice. Questo semplice codice non sarebbe adatto per un'applicazione di produzione effettiva.

- Il codice non garantisce che ogni messaggio della coda venga elaborato al massimo una volta. Quando si riceve un messaggio dalla coda, si verifica un periodo di timeout durante il quale il messaggio è invisibile ad altri listener della coda. Se il timeout scade prima che il messaggio venga eliminato, il messaggio diventa nuovamente visibile. Pertanto, se un'istanza del ruolo di lavoro impiega molto tempo per l'elaborazione di un messaggio, è teoricamente possibile che lo stesso messaggio venga elaborato due volte, ottenendo un'attività duplicata nel database. Per altre informazioni su questo problema, vedere [uso delle code di archiviazione di Azure](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- La logica di polling della coda può essere più conveniente, suddividendo in batch il recupero dei messaggi. Ogni volta che si chiama [CloudQueue. GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), è previsto un costo per la transazione. In alternativa, è possibile chiamare [CloudQueue. GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (prendere nota del plurale ' s'), che ottiene più messaggi in una singola transazione. I costi delle transazioni per le code di archiviazione di Azure sono molto bassi, quindi l'effetto sui costi non è sostanziale nella maggior parte degli scenari.
- Il loop stretto nel codice di elaborazione dei messaggi della coda causa l'affinità della CPU, che non usa in modo efficiente le VM multicore. Una progettazione migliore usa il parallelismo delle attività per eseguire diverse attività asincrone in parallelo.
- L'elaborazione dei messaggi della coda ha solo una gestione delle eccezioni rudimentale. Ad esempio, il codice non gestisce [i messaggi non elaborabili](https://msdn.microsoft.com/library/ms789028.aspx). Quando l'elaborazione del messaggio genera un'eccezione, è necessario registrare l'errore ed eliminare il messaggio. in caso contrario, il ruolo di lavoro tenterà di elaborarlo di nuovo e il ciclo continuerà per un periodo illimitato.

### <a name="sql-queries-are-unbounded"></a>Query SQL non associate

Il codice corrente di correzione it non impone alcun limite per il numero di righe che possono essere restituite dalle query per le pagine di indice. Se nel database viene immesso un volume elevato di attività, le dimensioni degli elenchi ottenuti potrebbero causare problemi di prestazioni. La soluzione consiste nell'implementare il paging. Per un esempio, vedere [ordinamento, filtro e paging con la Entity Framework in un'applicazione MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Modelli di visualizzazione consigliati

L'app Fix it usa la classe di entità FixItTask per passare le informazioni tra il controller e la visualizzazione. Una procedura consigliata consiste nell'usare i modelli di visualizzazione. Il modello di dominio, ad esempio la classe di entità FixItTask, è progettato in base a quanto necessario per la persistenza dei dati, mentre un modello di visualizzazione può essere progettato per la presentazione dei dati. Per altre informazioni, vedere [12 ASP.NET MVC Best Practices](https://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>BLOB di immagini protette consigliato

L'app Fix it archivia le immagini caricate come pubbliche, ovvero chiunque possa trovare l'URL può accedere alle immagini. Le immagini possono essere protette anziché pubbliche.

### <a name="no-powershell-automation-scripts-for-queues"></a>Nessun script di automazione di PowerShell per le code

Gli script di automazione di PowerShell di esempio sono stati scritti solo per la versione di base di correzione eseguita completamente in app Azure app Web del servizio. Non sono stati forniti script per la configurazione e la distribuzione nell'app Web e l'ambiente del servizio cloud necessario per l'elaborazione delle code.

### <a name="special-handling-for-html-codes-in-user-input"></a>Gestione speciale per i codici HTML nell'input utente

ASP.NET impedisce automaticamente molti modi in cui gli utenti malintenzionati potrebbero provare gli attacchi di scripting tra siti immettendo lo script nelle caselle di testo di input dell'utente. E l'helper `DisplayFor` MVC usato per visualizzare i titoli delle attività e le note codificano automaticamente i valori che invia al browser. Tuttavia, in un'app di produzione è consigliabile adottare misure aggiuntive. Per ulteriori informazioni, vedere la pagina relativa [alla convalida delle richieste in ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Procedure consigliate

Di seguito sono riportati alcuni problemi che sono stati risolti dopo essere stati individuati nella revisione del codice e i test della versione originale dell'app per la correzione. Alcune sono state causate dal fatto che il codificatore originale non è a conoscenza di una particolare procedura consigliata, alcune semplicemente perché il codice è stato scritto rapidamente e non era destinato al software rilasciato. In questo articolo vengono elencati i problemi in caso di elementi appresi da questa revisione e test che potrebbero essere utili ad altri utenti che sviluppano anche app Web.

### <a name="dispose-the-database-repository"></a>Eliminare il repository di database

La classe `FixItTaskRepository` deve eliminare l'istanza di `DbContext` Entity Framework. Questa operazione è stata apportata implementando `IDisposable` nella classe `FixItTaskRepository`:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Si noti che AutoFac eliminerà automaticamente l'istanza `FixItTaskRepository`, quindi non è necessario eliminarla in modo esplicito.

Un'altra opzione consiste nel rimuovere la variabile membro `DbContext` da `FixItTaskRepository`e creare invece una variabile `DbContext` locale all'interno di ogni metodo del repository, all'interno di un'istruzione `using`. Ad esempio,

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrare i singleton come tali con DI

Poiché è necessaria una sola istanza della classe `PhotoService` e della classe `Logger`, queste classi devono essere [registrate come istanze singole per l'inserimento di dipendenze](https://code.google.com/p/autofac/wiki/InstanceScope) in *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Sicurezza: non visualizzare i dettagli dell'errore per gli utenti

L'app per la correzione originale non disponeva di una pagina di errore generica e consente di visualizzare tutte le eccezioni fino all'interfaccia utente, pertanto alcune eccezioni come gli errori di connessione al database potrebbero comportare la visualizzazione di una traccia dello stack completa nel browser. Informazioni dettagliate sugli errori possono a volte facilitare gli attacchi da parte di utenti malintenzionati. La soluzione consiste nel registrare i dettagli dell'eccezione e visualizzare una pagina di errore per l'utente che non include i dettagli dell'errore. La correzione dell'app it era già in registrazione e per visualizzare una pagina di errore è stato aggiunto `<customErrors mode=On>` nel file Web. config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Per impostazione predefinita, in questo modo *Views\Shared\Error.cshtml* viene visualizzato per individuare eventuali errori. È possibile personalizzare *Error. cshtml* o creare una visualizzazione della pagina di errore personalizzata e aggiungere un attributo `defaultRedirect`. È inoltre possibile specificare diverse pagine di errore per errori specifici.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Sicurezza: consente solo la modifica di un'attività da un creatore

La pagina di indice del dashboard Mostra solo le attività create dall'utente connesso, ma un utente malintenzionato potrebbe creare un URL con un ID per l'attività di un altro utente. È stato aggiunto il codice in *DashboardController.cs* per restituire 404 in questo caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Non ingoiare eccezioni

L'app Fix it originale ha appena restituito null dopo la registrazione di un'eccezione risultante da una query SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

In questo modo, l'utente avrà l'aspetto dell'esito positivo della query, ma non ha restituito alcuna riga. La soluzione consiste nel generare nuovamente l'eccezione dopo l'intercettazione e la registrazione:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Intercettare tutte le eccezioni nei ruoli di lavoro

Eventuali eccezioni non gestite in un ruolo di lavoro provocheranno il riciclo della macchina virtuale, pertanto si desidera eseguire il wrapping di tutte le operazioni eseguite in un blocco try-catch e gestire tutte le eccezioni.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Specificare la lunghezza per le proprietà di stringa nelle classi di entità

Per visualizzare il codice semplice, la versione originale dell'app Fix it non ha specificato lunghezze per i campi dell'entità FixItTask e, di conseguenza, sono stati definiti come varchar (max) nel database. Di conseguenza, l'interfaccia utente accetta quasi tutte le quantità di input. Specificando i limiti delle lunghezze che si applicano sia all'input utente nella pagina Web che alla dimensione della colonna nel database:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Contrassegnare i membri privati come ReadOnly quando non si prevede di modificare

Ad esempio, nella classe `DashboardController` viene creata un'istanza di `FixItTaskRepository` e non si prevede che venga modificata, quindi è stata definita come [ReadOnly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Usare l'elenco. Any () anziché list. Count () &gt; 0

Se si è interessati al fatto che uno o più elementi in un elenco corrispondano ai criteri specificati, usare il metodo [any](https://msdn.microsoft.com/library/bb534972.aspx) , perché restituisce non appena viene trovato un elemento che consente di adattare i criteri, mentre il metodo `Count` deve sempre scorrere ogni elemento. Il file *index. cshtml* del dashboard includeva originariamente il codice seguente:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

L'operazione è stata modificata in questo modo:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generare URL nelle visualizzazioni MVC usando gli helper MVC

Per il pulsante **Crea un Fix it** nella Home page, l'app Fix it ha codificato un elemento Anchor:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Per i collegamenti di visualizzazione/azione di questo tipo è preferibile usare l'helper HTML [URL. Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) , ad esempio:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Usare Task. Delay anziché thread. Sleep nel ruolo di lavoro

Il modello nuovo-progetto inserisce `Thread.Sleep` nel codice di esempio per un ruolo di lavoro, ma la causa della sospensione del thread può causare la generazione di thread superflui aggiuntivi da parte del pool di thread. È possibile evitare tale operazione utilizzando [Task. Delay](https://msdn.microsoft.com/library/hh139096.aspx) .

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evitare il void asincrono

Se un metodo asincrono non deve restituire un valore, restituire un tipo di `Task` anziché `void`.

Questo esempio è relativo alla classe `FixItQueueManager`:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Usare `async void` solo per i gestori eventi di primo livello. Se si definisce un metodo come `async void`, il chiamante non può **attendere** il metodo o intercettare le eccezioni generate dal metodo. Per ulteriori informazioni, vedere [procedure consigliate nella programmazione asincrona](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Usare un token di annullamento per interrompere il ciclo del ruolo di lavoro

In genere, il metodo **Run** in un ruolo di lavoro contiene un ciclo infinito. Quando il ruolo di lavoro viene arrestato, viene chiamato il metodo [RoleEntryPoint. OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) . È consigliabile usare questo metodo per annullare le operazioni eseguite all'interno del metodo **Run** e uscire normalmente. In caso contrario, il processo potrebbe essere terminato nel corso di un'operazione.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Rifiutare esplicitamente la procedura di sniffing MIME automatico

In alcuni casi, Internet Explorer segnala un tipo MIME diverso dal tipo specificato dal server Web. Se, ad esempio, Internet Explorer rileva contenuto HTML in un file fornito con l'intestazione di risposta HTTP Content-Type: text/plain, Internet Explorer determina che il rendering del contenuto deve essere eseguito come HTML. Sfortunatamente, questo "analisi MIME" può causare problemi di sicurezza anche per i server che ospitano contenuto non attendibile. Per contrastare questo problema, Internet Explorer 8 ha apportato alcune modifiche al codice di determinazione del tipo MIME e consente agli sviluppatori di applicazioni di [rifiutare esplicitamente](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)l'analisi MIME. Il codice seguente è stato aggiunto al file *Web. config* .

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Abilitare la creazione di bundle e minification

Quando Visual Studio crea un nuovo progetto Web, la creazione di bundle e minification di file JavaScript non è abilitata per impostazione predefinita. È stata aggiunta una riga di codice in BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Impostare un timeout di scadenza per i cookie di autenticazione

Per impostazione predefinita, i cookie di autenticazione scadono entro due settimane. Un tempo più breve è più sicuro. È possibile modificare questa impostazione in *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Come eseguire l'app da Visual Studio nel computer locale

Esistono due modi per eseguire l'app Fix it:

- Eseguire l'applicazione di base che scrive nuove attività direttamente nel database SQL.
- Eseguire l'applicazione usando una coda e un servizio back-end per creare attività. Il modello di coda è descritto nel capitolo [modello di lavoro incentrato sulla coda](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Eseguire l'applicazione di base

1. Installare [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Installare [Azure SDK per .NET per Visual Studio](https://azure.microsoft.com/downloads/).
3. Scaricare il file zip da [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. In Esplora file fare clic con il pulsante destro del mouse sul file con estensione zip e scegliere Proprietà, quindi nella Finestra Proprietà fare clic su Sblocca.
5. Decomprimere il file.
6. Fare doppio clic sul file con estensione sln per avviare Visual Studio.
7. Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet**e quindi su **console di gestione pacchetti**.
8. Nella console di gestione pacchetti (PMC) fare clic su Ripristina.
9. Uscire da Visual Studio.
10. Avviare l' [emulatore di archiviazione di Azure](/azure/storage/common/storage-use-emulator).
11. Riavviare Visual Studio e aprire il file della soluzione chiuso nel passaggio precedente.
12. Verificare che il progetto FixIt sia impostato come progetto di avvio, quindi premere CTRL + F5 per eseguire il progetto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Eseguire l'applicazione con l'elaborazione della coda

1. Seguire le istruzioni per [eseguire l'applicazione di base](#runbase), quindi chiudere il browser e chiudere Visual Studio.
2. Avviare Visual Studio con privilegi di amministratore. Si userà l'emulatore di calcolo di Azure e che richiede privilegi di amministratore.
3. Nel file *Web. config* dell'applicazione nel progetto *MyFixIt* (il progetto Web), modificare il valore di `appSettings/UseQueues` su "true":

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Se l' [emulatore di archiviazione di Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) non è ancora in esecuzione, riavviarlo.
5. Eseguire simultaneamente il progetto Web FixIt e il progetto MyFixItCloudService.

    Uso di Visual Studio:

   1. Premere **F5** per eseguire il progetto Fixit.
   2. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto MyFixItCloudService, quindi scegliere **debug** > **Avvia nuova istanza**.

    Uso di Visual Studio 2013 Express per il Web:

   3. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione FixIt e scegliere **Proprietà**.
   4. Selezionare **progetti di avvio multipli**.
   5. Nell'elenco a discesa **azione** in MyFixIt e MyFixItCloudService selezionare **Avvia**.
   6. Fare clic su **OK**.
   7. Premere **F5** per eseguire entrambi i progetti.

      Quando si esegue il progetto MyFixItCloudService, Visual Studio avvia l'emulatore di calcolo di Azure. A seconda della configurazione del firewall, potrebbe essere necessario consentire l'emulatore tramite il firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Come distribuire l'app di base in app Web del servizio app Azure usando gli script di Windows PowerShell

Per illustrare il modello di [automazione di tutti gli elementi](automate-everything.md) , l'app Correggi it viene fornita con script che configurano un ambiente in Azure e distribuiscono il progetto nel nuovo ambiente. Nelle istruzioni seguenti viene illustrato come utilizzare gli script.

Se si vuole eseguire in Azure senza usare le code e sono state apportate le modifiche da eseguire localmente con le code, assicurarsi di impostare di nuovo il valore appSetting di UseQueues su false prima di procedere con le istruzioni seguenti.

Queste istruzioni presuppongono che sia già stato scaricato ed eseguito la soluzione Correggi it localmente e che si disponga di un account Azure o che si disponga di una sottoscrizione di Azure che si è autorizzati a gestire.

1. Installare la console di **Azure PowerShell** . Per istruzioni, vedere [Come installare e configurare Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Questa console personalizzata è configurata per l'uso con la sottoscrizione di Azure. Il modulo Azure viene installato nella directory *programmi* e viene importato automaticamente a ogni uso della console di Azure PowerShell.

    Se si preferisce lavorare in un programma host diverso, ad esempio Windows PowerShell ISE, assicurarsi di usare il cmdlet [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) per importare il modulo di Azure o usare un comando nel modulo di Azure per attivare l'importazione automatica del modulo.
2. Avviare Azure PowerShell con l'opzione **Esegui come amministratore** .
3. Eseguire il cmdlet [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) per impostare i criteri di esecuzione Azure PowerShell su `RemoteSigned`. Immettere **Y** (per Sì) per completare la modifica dei criteri.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Questa impostazione consente di eseguire script locali che non dispongono di una firma digitale. È anche possibile impostare i criteri di esecuzione su `Unrestricted`, in modo da eliminare la necessità di sbloccare il passaggio in un secondo momento, ma questa operazione non è consigliata per motivi di sicurezza.
4. Eseguire il cmdlet `Add-AzureAccount` per configurare PowerShell con le credenziali per l'account.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Queste credenziali scadono dopo un periodo di tempo ed è necessario eseguire nuovamente il cmdlet `Add-AzureAccount`. Poiché questo e-book viene scritto, il limite di tempo prima della scadenza delle credenziali è 12 ore.
5. Se si dispone di più sottoscrizioni, utilizzare il cmdlet Select-AzureSubscription per specificare la sottoscrizione in cui si desidera creare l'ambiente di test.
6. Importare un certificato di gestione per la stessa sottoscrizione di Azure usando i cmdlet `Get-AzurePublishSettingsFile` e `Import-AzurePublishSettingsFile`. Il primo di questi cmdlet Scarica un file di certificato e nel secondo si specifica il percorso del file per importarlo. > [!IMPORTANT]
   > Tenere il file scaricato in un percorso sicuro o eliminarlo al termine dell'operazione, perché contiene un certificato che può essere usato per gestire i servizi di Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Il certificato viene usato per una chiamata API REST che rileva l'indirizzo IP del computer di sviluppo per impostare una regola del firewall nel server di database SQL.
7. Eseguire il cmdlet [set-location](https://go.microsoft.com/fwlink/p/?linkid=293912) (gli alias sono `cd`, `chdir`e `sl`) per passare alla directory che contiene gli script. Si trovano nella cartella *automazione* nella cartella della soluzione Correggi it. Inserire il percorso tra virgolette se uno dei nomi di directory contiene spazi. Ad esempio, per passare alla directory `c:\Sample Apps\FixIt\Automation` è possibile immettere il comando seguente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Per consentire a Windows PowerShell di eseguire questi script, usare il cmdlet [Unblock-file](https://go.microsoft.com/fwlink/p/?linkid=294021) . Gli script sono bloccati perché sono stati scaricati da Internet.

    > [!WARNING]
    > Sicurezza: prima di eseguire `Unblock-File` in un file eseguibile o script, aprire il file nel blocco note, esaminare i comandi e verificare che non contengano codice dannoso.

    Ad esempio, il comando seguente esegue il cmdlet `Unblock-File` in tutti gli script della directory corrente.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Per creare l'app Web per la base (nessuna elaborazione delle code) correggerla, eseguire lo script di creazione dell'ambiente.

    Il parametro `Name` obbligatorio specifica il nome del database e viene usato anche per l'account di archiviazione creato dallo script. Il nome deve essere univoco a livello globale all'interno del dominio azurewebsites.net. Se si specifica un nome che non è univoco, ad esempio Fixit o test (o anche come nell'esempio fixitdemo), il `New-AzureWebsite` cmdlet ha esito negativo con un errore interno che segnala un conflitto. Lo script converte il nome in tutti i casi minuscoli in modo che siano conformi ai requisiti di nome per le app Web, gli account di archiviazione e i database.

    Il parametro obbligatorio `SqlDatabasePassword` specifica la password per l'account amministratore che verrà creato per il database SQL. Non includere caratteri XML speciali nella password (&amp; &lt; &gt;;). Si tratta di una limitazione del modo in cui gli script sono stati scritti, non di un limite di Azure.

    Se ad esempio si vuole creare un'app Web denominata "fixitdemo" e usare una password di amministratore SQL Server "Passw0rd1", è possibile immettere il comando seguente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Il nome deve essere univoco nel dominio azurewebsites.net e la password deve soddisfare i requisiti del database SQL per la complessità della password. Il Passw0rd1 di esempio soddisfa i requisiti.

    Si noti che il comando inizia con ".\". Per evitare l'esecuzione dannosa degli script, Windows PowerShell richiede di fornire il percorso completo del file di script quando si esegue uno script. È possibile usare un punto per indicare la directory corrente (".\") o specificare il percorso completo, ad esempio:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Per ulteriori informazioni sullo script, utilizzare il cmdlet `Get-Help`.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    È possibile usare i parametri `Detailed`, `Full`, `Parameters`e `Examples` del cmdlet Get-Help per filtrare la guida restituita.

    Se lo script ha esito negativo o genera errori, ad esempio "New-AzureWebsite: call set-AzureSubscription e Select-AzureSubscription First", è possibile che non sia stata completata la configurazione di Azure PowerShell.

    Al termine dello script, è possibile usare il portale di gestione di Azure per visualizzare le risorse create, come illustrato nel capitolo [automazione di tutti gli elementi](automate-everything.md) .
10. Per distribuire il progetto FixIt nel nuovo ambiente Azure, usare lo script *AzureWebsite. ps1* . Ad esempio,

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Al termine della distribuzione, il browser si apre con la correzione dell'esecuzione in Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Risoluzione dei problemi relativi agli script di Windows PowerShell

Gli errori più comuni rilevati durante l'esecuzione di questi script sono correlati alle autorizzazioni. Verificare che `Add-AzureAccount` e `Import-AzurePublishSettingsFile` abbiano avuto esito positivo e che siano stati usati per la stessa sottoscrizione di Azure. Anche se `Add-AzureAccount` ha avuto esito positivo, potrebbe essere necessario eseguirlo di nuovo. Le autorizzazioni aggiunte da `Add-AzureAccount` scadono entro 12 ore.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Riferimento a un oggetto non impostato su un'istanza di oggetto.

Se lo script restituisce errori, ad esempio "riferimento oggetto non impostato su un'istanza di un oggetto", il che significa che Windows PowerShell non è in grado di trovare un oggetto da elaborare (si tratta di un'eccezione di riferimento null), eseguire il cmdlet `Add-AzureAccount` e riprovare a eseguire lo script.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: si è verificato un errore interno del server.

Il cmdlet `New-AzureWebsite` restituisce un errore interno quando il nome non è univoco nel dominio azurewebsites.net. Per risolvere l'errore, usare un valore diverso per il nome, che è nel parametro Name di *New-AzureWebsiteEnv. ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Riavvio dello script

Se è necessario riavviare lo script *New-AzureWebsiteEnv. ps1* perché non è riuscito prima di stampare il messaggio "script completato", potrebbe essere necessario eliminare le risorse create dallo script prima dell'arresto. Ad esempio, se lo script ha già creato l'app Web ContosoFixItDemo e si esegue di nuovo lo script con lo stesso nome, lo script avrà esito negativo perché il nome è in uso.

Per determinare le risorse create dallo script prima dell'arresto, usare i cmdlet seguenti:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: per eseguire questo cmdlet, inviare tramite pipe il nome del server di database a `Get-AzureSqlDatabase`: `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Per eliminare queste risorse, usare i comandi seguenti. Si noti che se si elimina il server di database, vengono eliminati automaticamente i database associati al server.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Come distribuire l'app con l'elaborazione delle code in app Web del servizio app Azure e un servizio cloud di Azure

Per abilitare le code, apportare la modifica seguente nel file MyFixIt\Web.config. In `appSettings`modificare il valore di `UseQueues` su "true":

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Distribuire quindi l'applicazione MVC in un'app Web nel servizio app Azure, come descritto in [precedenza](#deploybase).

Successivamente, creare un nuovo servizio cloud di Azure. Gli script inclusi con l'app Correggi it non creano o distribuiscono il servizio cloud, pertanto è necessario usare portale di Azure a tale scopo. Nel portale fare clic su **nuovo** -- **calcolo** - **servizio cloud** -- **creazione rapida**, quindi immettere un URL e un percorso di Data Center. Usare lo stesso data center in cui è stata distribuita l'app Web.

![](the-fix-it-sample-application/_static/image1.png)

Prima di poter distribuire il servizio cloud, è necessario aggiornare alcuni file di configurazione.

In MyFixIt. WorkerRole\app.config, in `connectionStrings`, sostituire il valore della stringa di connessione `appdb` con la stringa di connessione effettiva per il database SQL. È possibile ottenere la stringa di connessione dal portale. Nel portale fare clic su **database sql** - **appdb** - **visualizzare le stringhe di connessione al database SQL per ADO .NET, ODBC, php e JDBC**. Copiare la stringa di connessione ADO.NET e incollare il valore nel file app. config. Sostituire "{Your\_password\_here}" con la password del database. (Supponendo che siano stati usati gli script per distribuire l'app MVC, è stata specificata la password del database nel parametro `SqlDatabasePassword` script).

Il risultato dovrebbe essere simile al seguente:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Nello stesso file MyFixIt. WorkerRole\app.config, in `appSettings`, sostituire i due valori segnaposto per l'account di archiviazione di Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

È possibile ottenere la chiave di accesso dal portale. Vedere [come gestire gli account di archiviazione](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

In MyFixItCloudService\ServiceConfiguration.Cloud.cscfg sostituire gli stessi due segnaposto valori per l'account di archiviazione di Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

A questo punto si è pronti per distribuire il servizio cloud. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto MyFixItCloudService e scegliere **pubblica**. Per ulteriori informazioni, vedere l'argomento relativo alla[distribuzione dell'applicazione in Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz), disponibile nella parte 2 di [questa esercitazione](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Precedente](more-patterns-and-guidance.md)
