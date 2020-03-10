---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitoraggio e telemetria (creazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: f61810ea7b486b2fa0bbb234edea7541eedde835
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583146"
---
# <a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitoraggio e telemetria (creazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Molte persone si affidano ai clienti per comunicare quando l'applicazione è inattiva. Non si tratta di una procedura consigliata, né soprattutto nel cloud. Non esiste alcuna garanzia di notifiche rapide e, quando si riceve una notifica, si ottengono spesso dati minimi o fuorvianti sugli eventi che si sono verificati. Con sistemi di registrazione e telemetria efficaci, è possibile essere a conoscenza di ciò che accade con l'app e quando si verificano problemi e si ha a disposizione informazioni utili per la risoluzione dei problemi.

## <a name="buy-or-rent-a-telemetry-solution"></a>Acquistare o affittare una soluzione di telemetria

> [!NOTE]
> Questo articolo è stato scritto prima del rilascio di [Application Insights](/azure/application-insights/app-insights-overview) . Application Insights è l'approccio preferito per le soluzioni di telemetria in Azure. Per altre informazioni, vedere [configurare Application Insights per il sito web ASP.NET](/azure/application-insights/app-insights-asp-net) .

Uno degli aspetti più interessanti dell'ambiente cloud è che è molto facile acquistare o affittare la strada alla vittoria. La telemetria è un esempio. Senza molto impegno è possibile ottenere un sistema di telemetria molto efficace e funzionante, molto conveniente. Ci sono numerosi partner interessanti che si integrano con Azure e alcuni di essi hanno livelli gratuiti, per cui è possibile ottenere dati di telemetria di base per niente. Di seguito sono riportate alcune di quelle attualmente disponibili in Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [DynaTrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) include anche le funzionalità di monitoraggio.

Si procederà rapidamente alla configurazione di New Relic per mostrare quanto sia facile usare un sistema di telemetria.

Nel portale di gestione di Azure iscriversi al servizio. Fare clic su **nuovo**e quindi su **Archivia**. Verrà visualizzata la finestra **di dialogo scegliere un componente** aggiuntivo. Scorrere verso il basso e fare clic su **New Relic**.

![Scegliere un componente aggiuntivo](monitoring-and-telemetry/_static/image1.png)

Fare clic sulla freccia a destra e scegliere il livello di servizio desiderato. Per questa demo verrà usato il livello gratuito.

![Personalizzare il componente aggiuntivo](monitoring-and-telemetry/_static/image2.png)

Fare clic sulla freccia destra, confermare che il "acquisto" e New Relic ora vengono visualizzati come componente aggiuntivo nel portale.

![Verifica acquisto](monitoring-and-telemetry/_static/image3.png)

![Componente aggiuntivo New Relic nel portale di gestione](monitoring-and-telemetry/_static/image4.png)

Fare clic su **informazioni di connessione**e copiare il codice di licenza.

![Informazioni di connessione](monitoring-and-telemetry/_static/image5.png)

Passare alla scheda **Configura** per l'app Web nel portale, impostare **monitoraggio delle prestazioni** su **componente**aggiuntivo e impostare l'elenco a discesa **Scegli componente aggiuntivo su** **nuovo Relic**. Fare quindi clic su **Salva**.

![Nuova reliquia nella scheda Configura](monitoring-and-telemetry/_static/image6.png)

In Visual Studio installare il pacchetto NuGet New Relic nell'app.

![Developer Analytics nella scheda Configura](monitoring-and-telemetry/_static/image7.png)

Distribuire l'app in Azure e iniziare a usarla. Creare alcune attività di correzione it per fornire alcune attività per il monitoraggio di New Relic.

Tornare quindi alla pagina **New Relic** nella scheda **componenti** aggiuntivi del portale e fare clic su **Gestisci**. Il portale invia il portale di gestione di New Relic, usando Single Sign-On per l'autenticazione, quindi non è necessario immettere di nuovo le credenziali. La pagina Overview presenta una serie di statistiche sulle prestazioni. (Fare clic sull'immagine per visualizzare le dimensioni massime della pagina Panoramica).

[![scheda monitoraggio New Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Di seguito sono riportate solo alcune delle statistiche che è possibile visualizzare:

- Tempo medio di risposta in momenti diversi del giorno.

    ![Tempo di risposta](monitoring-and-telemetry/_static/image10.png)
- Velocità effettiva (richieste al minuto) in momenti diversi del giorno.

    ![Velocità effettiva](monitoring-and-telemetry/_static/image11.png)
- Tempo CPU server dedicato alla gestione di richieste HTTP diverse.

    ![Tempi di transazione Web](monitoring-and-telemetry/_static/image12.png)
- Tempo CPU dedicato a diverse parti del codice dell'applicazione:

    ![Dettagli traccia](monitoring-and-telemetry/_static/image13.png)
- Statistiche sulle prestazioni cronologiche.

    ![Prestazioni cronologiche](monitoring-and-telemetry/_static/image14.png)
- Chiamate a servizi esterni, ad esempio il servizio BLOB e le statistiche sul modo in cui il servizio è stato affidabile e reattivo.

    ![Servizi esterni](monitoring-and-telemetry/_static/image15.png)

    ![Servizi esterni](monitoring-and-telemetry/_static/image16.png)

    ![Servizio esterno](monitoring-and-telemetry/_static/image17.png)
- Informazioni sul luogo in cui si è provenuto il traffico delle app Web negli Stati Uniti.

    ![Geografia](monitoring-and-telemetry/_static/image18.png)

È inoltre possibile impostare report ed eventi. È possibile, ad esempio, che ogni volta che si inizia a visualizzare gli errori, inviare un messaggio di posta elettronica al personale del supporto per gli avvisi al problema.

![Report](monitoring-and-telemetry/_static/image19.png)

New Relic è solo un esempio di sistema di telemetria. è possibile ottenere tutto questo anche da altri servizi. La bellezza del cloud consiste nel fatto che, senza dover scrivere codice e, per spese minime o no, è possibile ottenere un numero elevato di informazioni sull'utilizzo dell'applicazione e sui risultati effettivamente riscontrati dai clienti.

<a id="log"></a>
## <a name="log-for-insight"></a>Log per informazioni dettagliate

Un pacchetto di telemetria è un primo passaggio, ma è comunque necessario instrumentare il proprio codice. Il servizio di telemetria indica quando si verifica un problema e indica i risultati riscontrati dai clienti, ma potrebbe non fornire una grande quantità di informazioni su ciò che accade nel codice.

Non è necessario dover accedere in remoto a un server di produzione per vedere cosa sta facendo l'app. Questo potrebbe essere utile quando si dispone di un server, ma quando si è passati a centinaia di server e non si sa quali elementi è necessario eseguire in remoto? La registrazione deve fornire informazioni sufficienti che non è mai necessario eseguire in remoto nei server di produzione per analizzare ed eseguire il debug dei problemi. È necessario registrare informazioni sufficienti per isolare i problemi esclusivamente nei log.

### <a name="log-in-production"></a>Accedi alla produzione

Molti utenti attivano la traccia in produzione solo quando si verifica un problema e si desidera eseguire il debug. Ciò può comportare un ritardo sostanziale tra il momento in cui si è a conoscenza di un problema e il momento in cui si ottengono utili informazioni sulla risoluzione dei problemi. E le informazioni che si ottengono potrebbero non essere utili per gli errori intermittenti.

Ciò che si consiglia nell'ambiente cloud in cui l'archiviazione è conveniente è che si lascia sempre l'accesso in produzione. In questo modo, quando si verificano errori, è già stato effettuato l'accesso e sono disponibili dati cronologici che consentono di analizzare i problemi che si sviluppano nel tempo o che si verificano regolarmente in momenti diversi. È possibile automatizzare un processo di eliminazione per eliminare i log precedenti, ma potrebbe risultare più costoso configurare un processo di questo tipo rispetto al mantenimento dei log.

Il costo aggiunto per la registrazione è semplice rispetto alla quantità di tempo e denaro che è possibile salvare grazie alla disponibilità di tutte le informazioni necessarie quando si verificano problemi. Quindi, quando qualcuno indica che si è verificato un errore casuale a circa 8:00, ma non ricorda l'errore, è possibile individuare facilmente il problema.

Per meno di $4 al mese è possibile tenere a portata di mano 50 gigabyte di log e l'effetto sulle prestazioni della registrazione è molto semplice, purché si tenga presente una cosa, per evitare colli di bottiglia delle prestazioni, assicurarsi che la libreria di registrazione sia asincrona.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Differenziare i log che forniscono informazioni dai log che richiedono un'azione

I log sono destinati a informare (voglio sapere qualcosa) o agire (Desidero eseguire un'operazione). Prestare attenzione a scrivere solo log ACT per i problemi che richiedono effettivamente a un utente o a un processo automatizzato di agire. Troppi log ACT creeranno un rumore, richiedendo troppo lavoro per setacciare tutto per trovare i veri problemi. Se i log ACT attivano automaticamente alcune azioni, ad esempio l'invio di un messaggio di posta elettronica al personale del supporto, evitano che migliaia di tali azioni vengano attivate da un singolo problema.

Nella traccia di .NET System. Diagnostics, ai log è possibile assegnare un livello di errore, avviso, informazioni e Debug/Verbose. È possibile distinguere ACT da informa log riservando il livello di errore per i log ACT e usando i livelli inferiori per i log di informazioni.

![Livelli di registrazione](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurare i livelli di registrazione in fase di esecuzione

Sebbene valga la pena avere la registrazione sempre attiva nell'ambiente di produzione, un'altra procedura consigliata consiste nell'implementare un Framework di registrazione che consenta di modificare in fase di esecuzione il livello di dettaglio che si sta registrando, senza ridistribuire o riavviare l'applicazione. Ad esempio, quando si utilizza la funzionalità di traccia in `System.Diagnostics` è possibile creare log di errore, avviso, informazioni e Debug/Verbose. Si consiglia di registrare sempre log di errore, di avviso e di informazioni nell'ambiente di produzione e si desidera essere in grado di aggiungere in modo dinamico la registrazione debug/verbose per la risoluzione dei problemi in base al caso.

Le app Web nel servizio app Azure includono il supporto incorporato per la scrittura di log `System.Diagnostics` nell'file system, nell'archiviazione tabelle o nell'archivio BLOB. È possibile selezionare diversi livelli di registrazione per ogni destinazione di archiviazione ed è possibile modificare il livello di registrazione in tempo reale senza riavviare l'applicazione. Il supporto dell'archiviazione BLOB semplifica l'esecuzione dei processi di analisi di [HDInsight](https://docs.microsoft.com/azure/hdinsight/) nei log dell'applicazione, perché HDInsight sa come lavorare direttamente con l'archivio BLOB.

### <a name="log-exceptions"></a>Registrare eccezioni

Non solo inserire *un'eccezione. ToString ()* nel codice di registrazione. Che lascia le informazioni contestuali. Nel caso di errori SQL, viene lasciato il numero di errore SQL. Per tutte le eccezioni, includere le informazioni sul contesto, l'eccezione stessa e le eccezioni interne per assicurarsi di fornire tutti gli elementi che saranno necessari per la risoluzione dei problemi. Ad esempio, le informazioni sul contesto potrebbero includere il nome del server, un identificatore di transazione e un nome utente, ma non la password o i segreti.

Se ci si affida a ogni sviluppatore per eseguire le operazioni corrette con la registrazione delle eccezioni, alcune di esse non verranno eseguite. Per assicurarsi che venga eseguita in modo corretto ogni volta, compilare la gestione delle eccezioni direttamente nell'interfaccia del logger: passare l'oggetto eccezione alla classe logger e registrare correttamente i dati dell'eccezione nella classe logger.

### <a name="log-calls-to-services"></a>Chiamate di log ai servizi

Si consiglia vivamente di scrivere un log ogni volta che l'app viene chiamata a un servizio, sia che si tratti di un database o di un'API REST o di un servizio esterno. Includere nei log non solo un'indicazione di esito positivo o negativo, ma il tempo richiesto per ogni richiesta. Nell'ambiente cloud spesso si riscontrano problemi relativi a rallentamenti anziché a interruzioni. Un elemento che normalmente impiega 10 millisecondi potrebbe iniziare a richiedere un secondo. Quando qualcuno indica che l'app è lenta, è opportuno poter esaminare New Relic o il servizio di telemetria che si ha e convalidare la propria esperienza, quindi si vuole essere in grado di esaminare i log per approfondire i dettagli del motivo per cui è lento.

### <a name="use-an-ilogger-interface"></a>Usare un'interfaccia ILogger

Ciò che si consiglia di eseguire quando si crea un'applicazione di produzione consiste nel creare una semplice interfaccia *ILogger* e intenervi alcuni metodi. In questo modo è più semplice modificare l'implementazione della registrazione in un secondo momento e non dover passare a tutto il codice per eseguire questa operazione. Si potrebbe usare la classe `System.Diagnostics.Trace` nell'app Correggi it, ma è possibile usarla sotto le quinte in una classe di registrazione che implementa *ILogger*e le chiamate al metodo *ILogger* vengono effettuate nell'app.

In questo modo, se si desidera rendere più ricca la registrazione, è possibile sostituire [`System.Diagnostics.Trace`](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) con qualsiasi meccanismo di registrazione desiderato. Ad esempio, man mano che l'app cresce, è possibile decidere di usare un pacchetto di registrazione più completo, ad esempio [NLog](http://nlog-project.org/) o [Enterprise Library Logging Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). [Log4net](http://logging.apache.org/log4net/) è un altro framework di registrazione comune, ma non esegue la registrazione asincrona.

Una delle possibili cause per l'uso di un Framework, ad esempio NLog, consiste nel semplificare la suddivisione dell'output di registrazione in archivi dati a volume elevato e ad alto valore distinti. Questo consente di archiviare in modo efficiente volumi elevati di dati di notifica che non sono necessari per eseguire query veloci, mantenendo al tempo stesso l'accesso rapido ai dati di ACT.

### <a name="semantic-logging"></a>Registrazione semantica

Per un modo relativamente nuovo per eseguire la registrazione che può produrre informazioni di diagnostica più utili, vedere [Enterprise Library Semantic Logging Application Block (lastra)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). LASTRA USA [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) e il supporto [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) in .NET 4,5 per consentire la creazione di log più strutturati e Queryable. Si definisce un metodo diverso per ogni tipo di evento che si registra, che consente di personalizzare le informazioni scritte. Per registrare un errore del database SQL, ad esempio, è possibile chiamare un metodo di `LogSQLDatabaseError`. Per questo tipo di eccezione, si sa che il numero di errore è un numero di informazioni chiave, pertanto è possibile includere un parametro di numero di errore nella firma del metodo e registrare il numero di errore come campo separato nel record di log che si scrive. Poiché il numero si trova in un campo separato, è possibile ottenere in modo più semplice e affidabile i report in base ai numeri di errore SQL che non si può fare se il numero di errore viene semplicemente concatenato in una stringa di messaggio.

## <a name="logging-in-the-fix-it-app"></a>Registrazione nell'app Correggi it

### <a name="the-ilogger-interface"></a>Interfaccia ILogger

Di seguito è illustrata l'interfaccia *ILogger* nell'app Correggi it.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Questi metodi consentono di scrivere log con gli stessi quattro livelli supportati da *System. Diagnostics*. I metodi TraceApi sono per la registrazione di chiamate al servizio esterno con informazioni sulla latenza. È anche possibile aggiungere un set di metodi per il livello Debug/Verbose.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Implementazione del logger dell'interfaccia ILogger

L'implementazione dell'interfaccia è molto semplice. In pratica, viene semplicemente chiamato nei metodi *System. Diagnostics* standard. Nel frammento di codice seguente vengono illustrati tutti e tre i metodi informativi e uno degli altri.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Chiamata dei metodi ILogger

Ogni volta che il codice nell'app Correggi it rileva un'eccezione, viene chiamato un metodo *ILogger* per registrare i dettagli dell'eccezione. Ogni volta che effettua una chiamata al database, al servizio BLOB o a un'API REST, avvia un cronometro prima della chiamata, arresta il cronometro quando viene restituito il servizio e registra il tempo trascorso con le informazioni sull'esito positivo o negativo.

Si noti che il messaggio del log include il nome della classe e il nome del metodo. È consigliabile assicurarsi che i messaggi di log identifichino la parte del codice dell'applicazione scritta.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Quindi, ora per ogni volta che l'app Fix it ha effettuato una chiamata al database SQL, è possibile visualizzare la chiamata, il metodo che lo ha chiamato e esattamente il tempo impiegato.

![Query del database SQL nei log](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Se si esaminano i log, è possibile notare che il tempo necessario per le chiamate del database è variabile. Queste informazioni potrebbero essere utili: poiché l'app registra tutti gli eventi, è possibile analizzare le tendenze cronologiche della modalità di esecuzione del servizio di database nel tempo. Un servizio, ad esempio, potrebbe essere rapido per la maggior parte del tempo, ma le richieste potrebbero non riuscire oppure le risposte potrebbero rallentare in determinati orari del giorno.

È possibile eseguire la stessa operazione per il servizio BLOB: per ogni volta che l'app carica un nuovo file, è presente un log ed è possibile vedere esattamente il tempo impiegato per caricare ogni file.

![Log di caricamento BLOB](monitoring-and-telemetry/_static/image23.png)

Si tratta solo di un paio di righe di codice da scrivere ogni volta che si chiama un servizio e ora ogni volta che qualcuno afferma che si è verificato un problema, si conosce esattamente la causa del problema, se si tratta di un errore o anche se è stato eseguito lentamente. È possibile individuare l'origine del problema senza dover accedere in remoto a un server o attivare la registrazione dopo che si è verificata l'errore e sperare di crearla di nuovo.

## <a name="dependency-injection-in-the-fix-it-app"></a>Inserimento delle dipendenze nell'app Correggi it

Ci si potrebbe chiedere come il costruttore del repository nell'esempio illustrato in precedenza ottenga l'implementazione dell'interfaccia del logger:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Per collegare l'interfaccia all'implementazione, l'app usa l' [inserimento delle dipendenze](http://en.wikipedia.org/wiki/Dependency_injection)(di) con [AutoFac](http://autofac.org/). Consente di utilizzare un oggetto basato su un'interfaccia in molte posizioni in tutto il codice e di specificare solo in un'unica posizione l'implementazione che viene utilizzata quando viene creata un'istanza dell'interfaccia. In questo modo è più semplice modificare l'implementazione. ad esempio, potrebbe essere necessario sostituire il Logger System. Diagnostics con un logger NLog. O per i test automatizzati, potrebbe essere necessario sostituire una versione fittizia del logger.

L'applicazione Correggi it utilizza l'inserimento di dipendenze in tutti i repository e tutti i controller. I costruttori delle classi controller ottengono un'interfaccia *ITaskRepository* nello stesso modo in cui il repository ottiene un'interfaccia logger:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

L'app usa la libreria AutoFac di per fornire automaticamente le istanze di *TaskRepository* e *logger* per questi costruttori.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Questo codice dice fondamentalmente che ovunque un costruttore necessita di un'interfaccia *ILogger* , passare un'istanza della classe *logger* e, ogni volta che è necessaria un'interfaccia *IFixItTaskRepository* , passare un'istanza della classe *FixItTaskRepository* .

[AutoFac](http://autofac.org/) è uno dei numerosi framework di inserimento delle dipendenze che è possibile usare. Un altro comune è [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), che è consigliato e supportato da Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Supporto per la registrazione incorporata in Azure

Azure supporta i tipi di [registrazione seguenti per le app Web nel servizio app Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Traccia System. Diagnostics (è possibile attivare e disattivare e impostare i livelli in tempo reale senza riavviare il sito).
- Eventi di Windows.
- Log di IIS (HTTP/FREB).

Azure supporta i tipi di [registrazione seguenti nei servizi cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Traccia System. Diagnostics.
- Contatori delle prestazioni.
- Eventi di Windows.
- Log di IIS (HTTP/FREB).
- Monitoraggio della directory personalizzata.

L'app Correggi it usa la traccia System. Diagnostics. Per abilitare la registrazione di System. Diagnostics in un'app Web, è sufficiente capovolgere un'opzione nel portale o chiamare l'API REST. Nel portale fare clic sulla scheda **configurazione** per il sito e scorrere verso il basso per visualizzare la sezione **diagnostica applicazioni** . È possibile attivare o disattivare la registrazione e selezionare il livello di registrazione desiderato. È possibile fare in modo che Azure scriva i log nel file system o in un account di archiviazione.

![Diagnostica dell'app e diagnostica del sito nella scheda Configura](monitoring-and-telemetry/_static/image24.png)

Dopo aver abilitato la registrazione in Azure, è possibile visualizzare i log nella finestra di output di Visual Studio appena vengono creati.

![Menu dei log di streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu dei log di streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

È anche possibile avere log scritti nell'account di archiviazione e visualizzarli con qualsiasi strumento in grado di accedere al servizio tabelle di archiviazione di Azure, ad esempio **Esplora server** in Visual Studio o [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Log in Esplora server](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Riepilogo

È molto semplice implementare un sistema di telemetria predefinito, instrumentare la registrazione nel proprio codice e configurare la registrazione in Azure. Quando si verificano problemi di produzione, la combinazione di un sistema di telemetria e di log personalizzati consentirà di risolvere rapidamente i problemi prima che diventino principali problemi per i clienti.

Nel [prossimo capitolo](transient-fault-handling.md) verrà illustrato come gestire gli errori temporanei in modo che non diventino problemi di produzione da analizzare.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere le seguenti risorse.

Documentazione principalmente sui dati di telemetria:

- [Modelli e procedure Microsoft: informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere linee guida sulla strumentazione e la telemetria, indicazioni sull'analisi del servizio, modello di monitoraggio degli endpoint di integrità e modello di riconfigurazione del runtime.
- [Un centesimo sul cloud: abilitazione del monitoraggio delle prestazioni di New Relic in siti Web di Azure](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi cloud di Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper di Mark Simms e Michael Thomassy. Vedere la sezione telemetria e diagnostica.
- [Sviluppo di prossima generazione con Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Articolo di MSDN Magazine.

Documentazione principalmente sulla registrazione:

- [Blocco di applicazioni per la registrazione semantica (lastra)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie presenta il caso per la registrazione semantica con lastra.
- [Creazione di log strutturati e significativi con la registrazione semantica](https://channel9.msdn.com/Events/Build/2013/3-336). Video Julian Dominguez presenta il caso per la registrazione semantica con lastra.
- [Registrazione SQL EF6-parte 1: registrazione semplice](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers illustra come registrare le query eseguite da Entity Framework in EF 6.
- [Resilienza della connessione e intercettazione dei comandi con il Entity Framework in un'applicazione MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quarto in una serie di esercitazioni in nove parti, Mostra come usare la funzionalità di intercettazione dei comandi EF 6 per registrare i comandi SQL inviati al database da Entity Framework.
- [Migliorare la registrazione C# usando gli attributi delle informazioni sul chiamante 5,0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Come registrare facilmente il nome del metodo chiamante senza impostarlo come hardcoded in valori letterali o mediante reflection per ottenerlo manualmente.

Documentazione principalmente sulla risoluzione dei problemi:

- [Blog sulla risoluzione dei problemi di &amp; debug di Azure](https://blogs.msdn.com/b/kwill/).
- [AzureTools: utilità di diagnostica usata dal team di supporto tecnico Developer di Azure](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Introduce e fornisce un collegamento di download per uno strumento che può essere usato in una macchina virtuale di Azure per scaricare ed eseguire un'ampia gamma di strumenti di diagnostica e monitoraggio. Utile quando è necessario diagnosticare un problema in una determinata macchina virtuale.
- [Risolvere i problemi di un'app Web nel servizio app Azure usando Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Un'esercitazione dettagliata per iniziare a usare la traccia di System. Diagnostics e il debug remoto.

Video

- [Failsafe: compilazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Presenta concetti di alto livello e principi architetturali in modo molto accessibile e interessante, con storie tratte dall'esperienza del team di consulenza clienti Microsoft (CAT) con i clienti effettivi. Gli episodi 4 e 9 sono relativi al monitoraggio e alla telemetria. L'episodio 9 include una panoramica dei servizi di monitoraggio MetricsHub, AppDynamics, New Relic e PagerDuty.
- [Creazione di grandi dimensioni: lezioni apprese dai clienti di Azure-parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms parla di progettazione per l'errore e la strumentazione di tutto. In modo analogo alla serie failsafe, ma passa ad altre procedure dettagliate.

Esempio di codice:

- [Nozioni fondamentali sui servizi cloud in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Applicazione di esempio creata dal team di consulenza clienti di Microsoft Azure. Illustra i dati di telemetria e le procedure di registrazione, come illustrato negli articoli seguenti. L'esempio implementa la registrazione delle applicazioni usando [NLog](http://nlog-project.org/). Per la documentazione correlata, vedere la [serie di quattro articoli wiki TechNet sui dati di telemetria e la registrazione](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Precedente](design-to-survive-failures.md)
> [Successivo](transient-fault-handling.md)
