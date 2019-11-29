---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Modello di lavoro incentrato sulle code (compilazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: c73b070f11366e781bcea70ffc84fd49a47d469a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582755"
---
# <a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Modello di lavoro incentrato sulle code (compilazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

In precedenza, abbiamo notato che l'uso di più servizi può comportare un contratto di servizio "composto", in cui il contratto di servizio effettivo dell'app è il *prodotto* dei singoli contratti di servizio. Ad esempio, l'app Correggi it usa siti Web, archiviazione e database SQL. Se uno di questi servizi ha esito negativo, l'app restituirà un errore all'utente.

La memorizzazione nella cache è un modo efficace per gestire gli errori temporanei per il contenuto di sola lettura. Ma cosa accade se l'applicazione deve lavorare? Ad esempio, quando l'utente invia una nuova attività Correggi it, l'app non può semplicemente inserire l'attività nella cache. L'app deve scrivere l'attività Correggi it in un archivio dati persistente, in modo che possa essere elaborata.

Ecco dove entra il modello di lavoro incentrato sulle code. Questo modello consente l'accoppiamento libero tra un livello Web e un servizio back-end.

Ecco come funziona il modello. Quando l'applicazione riceve una richiesta, inserisce un elemento di lavoro su una coda e restituisce immediatamente la risposta. Un processo back-end separato estrae gli elementi di lavoro dalla coda ed esegue il lavoro.

Il modello di lavoro incentrato sulle code è utile per:

- Lavoro che richiede molto tempo (latenza elevata).
- Lavoro che richiede un servizio esterno che potrebbe non essere sempre disponibile.
- Lavoro con utilizzo intensivo delle risorse (CPU elevata).
- Lavoro che trarrebbe vantaggio dal livellamento della frequenza (soggetto a picchi improvvisi di carico).

## <a name="reduced-latency"></a>Latenza ridotta

Le code sono utili ogni volta che si esegue un lavoro che richiede molto tempo. Se un'attività richiede alcuni secondi o più, anziché bloccare l'utente finale, inserire l'elemento di lavoro in una coda. Indicare all'utente che si sta lavorando, quindi usare un listener della coda per elaborare l'attività in background.

Ad esempio, quando si acquista un prodotto in un rivenditore online, il sito Web conferma immediatamente l'ordine. Tuttavia, ciò non significa che il materiale è già presente in un camion da consegnare. Inseriscono un'attività in una coda e in background eseguono la verifica del credito, preparano gli elementi per la spedizione e così via.

Per gli scenari con latenza breve, l'ora end-to-end totale potrebbe essere più lunga utilizzando una coda, rispetto all'esecuzione sincrona dell'attività. Ma anche in questo modo, gli altri vantaggi possono superare lo svantaggio.

## <a name="increased-reliability"></a>Maggiore affidabilità

Nella versione di correggiamo che abbiamo esaminato finora, il front-end Web è strettamente collegato al back-end del database SQL. Se il servizio di database SQL non è disponibile, l'utente riceve un errore. Se i tentativi non funzionano, ovvero se l'errore è più che temporaneo, l'unica cosa che è possibile fare è visualizzare un errore e richiedere all'utente di riprovare più tardi.

![Diagramma che mostra il front-end Web in caso di errore del back-end del database SQL](queue-centric-work-pattern/_static/image1.png)

Usando le code, quando un utente invia un'attività Correggi, l'app scrive un messaggio nella coda. Il payload del messaggio è una rappresentazione [JSON](http://json.org/) dell'attività. Non appena il messaggio viene scritto nella coda, l'app restituisce e visualizza immediatamente un messaggio di operazione riuscita per l'utente.

Se uno dei servizi back-end, ad esempio il database SQL o il listener della coda, passa alla modalità offline, gli utenti possono comunque inviare nuove attività IT di correzione. I messaggi verranno accodati solo fino a quando i servizi back-end non saranno nuovamente disponibili. A questo punto, i servizi back-end rileveranno il backlog.

![Diagramma che mostra il front-end Web che continua a funzionare quando si verifica un errore del database SQL](queue-centric-work-pattern/_static/image2.png)

Ora è inoltre possibile aggiungere una logica di back-end senza preoccuparsi della resilienza del front-end. Ad esempio, è possibile inviare un messaggio di posta elettronica o SMS al proprietario ogni volta che viene assegnata una nuova correzione. Se il messaggio di posta elettronica o il servizio SMS non è più disponibile, è possibile elaborare tutti gli altri elementi, quindi inserire un messaggio in una coda separata per l'invio di messaggi di posta elettronica o SMS.

In precedenza, il contratto di SLA effettivo era app Web &times; archiviazione &times; database SQL = 99,7%. (Vedere [progettazione per sopravvivere agli errori](design-to-survive-failures.md)).

Quando si modifica l'app per l'uso di una coda, il front-end Web dipende solo da app Web e archiviazione, per un contratto di contratto del 99,8%. Si noti che le code fanno parte del servizio di archiviazione di Azure, quindi sono incluse nello stesso contratto di servizio dell'archiviazione BLOB.

Se è necessario un miglioramento pari al 99,8%, è possibile creare due code in due aree diverse. Designarne uno come primario e l'altro come secondario. Nell'app eseguire il failover nella coda secondaria se la coda primaria non è disponibile. La possibilità che entrambe le volte siano disponibili contemporaneamente è molto piccola.

## <a name="rate-leveling-and-independent-scaling"></a>Livellamento della frequenza e scalabilità indipendente

Le code sono utili anche per qualcosa denominato *livellamento della frequenza* o *livellamento del carico*.

Le app Web sono spesso soggette a picchi improvvisi nel traffico. Sebbene sia possibile usare la scalabilità automatica per aggiungere automaticamente server Web per gestire un aumento del traffico Web, la scalabilità automatica potrebbe non essere in grado di rispondere abbastanza rapidamente per gestire picchi improvvisi di carico. Se i server Web sono in grado di eseguire l'offload di alcune operazioni che è necessario eseguire scrivendo un messaggio in una coda, possono gestire più traffico. Un servizio back-end può quindi leggere i messaggi dalla coda ed elaborarli. Il livello di profondità della coda aumenterà o verrà ridotto in base alla variazione del carico in ingresso.

Con la maggior parte del lavoro dispendioso in termini di tempo in un servizio back-end, il livello Web può rispondere più facilmente ai picchi improvvisi del traffico. Puoi risparmiare denaro perché qualsiasi quantità di traffico può essere gestita da un numero inferiore di server Web.

Il livello Web e il servizio back-end possono essere ridimensionati in modo indipendente. Potrebbero ad esempio essere necessari tre server Web, ma solo un server di elaborazione della coda dei messaggi. In alternativa, se si esegue un'attività a elevato utilizzo di calcolo in background, potrebbe essere necessario un maggior numero di server back-end.

![](queue-centric-work-pattern/_static/image3.png)

La scalabilità automatica funziona con i servizi back-end e con il livello Web. È possibile aumentare o ridurre il numero di macchine virtuali che elaborano le attività nella coda, in base all'utilizzo della CPU delle VM back-end. In alternativa, è possibile applicare la scalabilità automatica in base al numero di elementi presenti in una coda. Ad esempio, è possibile indicare a scalabilità automatica di provare a non contenere più di 10 elementi della coda. Se la coda contiene più di 10 elementi, la scalabilità automatica aggiungerà macchine virtuali. Quando si aggiornano, la scalabilità automatica eliminerà le VM aggiuntive.

## <a name="adding-queues-to-the-fix-it-application"></a>Aggiunta di code all'applicazione Fix it

Per implementare il modello di coda, è necessario apportare due modifiche all'app Correggi it.

- Quando un utente invia una nuova attività Correggi, inserire l'attività nella coda anziché scriverla nel database.
- Creare un servizio back-end che elabora i messaggi nella coda.

Per la coda verrà usato il servizio di [archiviazione code di Azure](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Un'altra opzione consiste nell'usare il [bus di servizio di Azure](https://docs.microsoft.com/azure/service-bus/).

Per decidere il servizio di Accodamento da usare, considerare il modo in cui l'app deve inviare e ricevere i messaggi nella coda:

- Se si dispone di produttori cooperanti e consumer concorrente, provare a usare il servizio di archiviazione di Accodamento di Azure. "Produttori cooperanti" significa che più processi aggiungono messaggi a una coda. "Consumer concorrente" significa che più processi tirano i messaggi fuori dalla coda per elaborarli, ma qualsiasi messaggio può essere elaborato solo da un "consumer". Se è necessaria una velocità effettiva maggiore di quella che è possibile ottenere con una singola coda, usare code e/o account di archiviazione aggiuntivi.
- Se è necessario un [modello di pubblicazione/sottoscrizione](http://en.wikipedia.org/wiki/Publish/subscribe), provare a usare le code del bus di servizio di Azure.

Il Fix it app è idoneo per i produttori cooperanti e il modello di consumer concorrente.

Un'altra considerazione riguarda la disponibilità dell'applicazione. Il servizio di archiviazione di Accodamento fa parte dello stesso servizio usato per l'archiviazione BLOB, quindi l'uso di questo servizio non ha alcun effetto sul contratto di servizio. Il bus di servizio di Azure è un servizio separato con il proprio contratto di servizio. Se sono state usate le code del bus di servizio, è necessario calcolare una percentuale di SLA aggiuntiva e il contratto di servizio composito sarebbe più basso. Quando si sceglie un servizio di Accodamento, assicurarsi di comprendere l'effetto della propria scelta sulla disponibilità dell'applicazione. Per ulteriori informazioni, vedere la sezione [risorse](#resources) .

## <a name="creating-queue-messages"></a>Creazione di messaggi in coda

Per inserire un'attività Correggi it nella coda, il front-end Web esegue i passaggi seguenti:

1. Creare un'istanza di [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) . L'istanza di `CloudQueueClient` viene utilizzata per eseguire le richieste nel servizio di Accodamento.
2. Creare la coda, se non esiste già.
3. Serializzare l'attività Correggi it.
4. Chiamare [CloudQueue. AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) per inserire il messaggio nella coda.

Questa operazione verrà eseguita nel costruttore e `SendMessageAsync` metodo di una nuova classe `FixItQueueManager`.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Qui viene usata la libreria [JSON.NET](https://github.com/JamesNK/Newtonsoft.Json) per serializzare il Fixit in formato JSON. Puoi usare qualsiasi approccio di serializzazione che preferisci. JSON ha il vantaggio di essere leggibile, pur essendo meno dettagliato del codice XML.

Il codice di qualità di produzione aggiungerebbe la logica di gestione degli errori, Sospendi se il database non è più disponibile, gestisce il ripristino in modo più pulito, crea la coda all'avvio dell'applicazione e gestisce[i messaggi "non elaborabili"](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). Un messaggio non elaborabile è un messaggio che non può essere elaborato per qualche motivo. Non si vuole che i messaggi non elaborabili si trovino nella coda, in cui il ruolo di lavoro tenterà continuamente di elaborarli, non riuscire, riprovare, non riuscire e così via.

Nell'applicazione MVC front-end è necessario aggiornare il codice che crea una nuova attività. Anziché inserire l'attività nel repository, chiamare il metodo `SendMessageAsync` illustrato in precedenza.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Elaborazione dei messaggi della coda

Per elaborare i messaggi nella coda, verrà creato un servizio back-end. Il servizio back-end eseguirà un ciclo infinito che esegue i passaggi seguenti:

1. Ottiene il messaggio successivo dalla coda.
2. Deserializzare il messaggio in un'attività Correggi it.
3. Scrivere l'attività Correggi it nel database.

Per ospitare il servizio back-end, verrà creato un servizio cloud di Azure che contiene un *ruolo di lavoro*. Un ruolo di lavoro è costituito da una o più macchine virtuali che possono eseguire l'elaborazione del back-end. Il codice in esecuzione in queste VM effettuerà il pull dei messaggi dalla coda man mano che diventano disponibili. Per ogni messaggio verrà deserializzato il payload JSON e si scriverà un'istanza dell'entità dell'attività Correggi it nel database, usando lo stesso repository usato in precedenza nel livello Web.

Nei passaggi seguenti viene illustrato come aggiungere un progetto di ruolo di lavoro a una soluzione che dispone di un progetto Web standard. Questi passaggi sono già stati eseguiti nel progetto Correggi it che è possibile scaricare.

Aggiungere prima di tutto un progetto di servizio cloud alla soluzione Visual Studio. Fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Aggiungi**, quindi **nuovo progetto**. Nel riquadro sinistro espandere oggetto **visivo C#**  e selezionare **cloud**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

Nella finestra di dialogo **nuovo servizio cloud di Azure** espandere il nodo  **C# visivo** nel riquadro sinistro. Selezionare **ruolo di lavoro** e fare clic sull'icona a forma di freccia destra.

![](queue-centric-work-pattern/_static/image6.png)

Si noti che è anche possibile aggiungere un *ruolo Web*. È possibile eseguire Fix it front-end nello stesso servizio cloud anziché eseguirlo in un sito Web di Azure. Questa operazione offre alcuni vantaggi per semplificare la coordinazione delle connessioni tra front-end e back-end. Tuttavia, per mantenere la demo semplice, il front-end viene mantenuto in un'app Web del servizio app Azure e viene eseguito solo il back-end in un servizio cloud.

Un nome predefinito viene assegnato al ruolo di lavoro. Per modificare il nome, posizionare il puntatore del mouse sul ruolo di lavoro nel riquadro destro, quindi fare clic sull'icona a forma di matita.

![](queue-centric-work-pattern/_static/image7.png)

Fare clic su **OK** per completare la finestra di dialogo. In questo modo vengono aggiunti due progetti alla soluzione Visual Studio.

- progetto Azure che definisce il servizio cloud, incluse le informazioni di configurazione.
- Progetto di ruolo di lavoro che definisce il ruolo di lavoro.

![](queue-centric-work-pattern/_static/image8.png)

Per altre informazioni, vedere [creazione di un progetto Azure con Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

All'interno del ruolo di lavoro, viene eseguito il polling dei messaggi chiamando il metodo `ProcessMessageAsync` della classe `FixItQueueManager` che abbiamo visto in precedenza.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

Il metodo `ProcessMessagesAsync` controlla se è presente un messaggio in attesa. Se ne esiste uno, deserializza il messaggio in un'entità `FixItTask` e salva l'entità nel database. Il ciclo viene eseguito fino a quando la coda non è vuota.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Il polling dei messaggi della coda comporta un piccolo addebito per le transazioni, quindi, quando non è presente alcun messaggio in attesa di essere elaborato, il metodo di `RunAsync` del ruolo di lavoro attende un secondo prima di eseguire di nuovo il polling chiamando `Task.Delay(1000)`.

In un progetto Web l'aggiunta di codice asincrono può migliorare automaticamente le prestazioni perché IIS gestisce un pool di thread limitato. Questo non avviene in un progetto di ruolo di lavoro. Per migliorare la scalabilità del ruolo di lavoro, è possibile scrivere codice multithread o usare codice asincrono per implementare la [programmazione parallela](https://msdn.microsoft.com/library/ff963553.aspx). L'esempio non implementa la programmazione parallela, ma Mostra come rendere il codice asincrono per poter implementare la programmazione parallela.

## <a name="summary"></a>Riepilogo

In questo capitolo è stato illustrato come migliorare la velocità di risposta, l'affidabilità e la scalabilità dell'applicazione implementando il modello di lavoro incentrato sulle code.

Questo è l'ultimo dei 13 modelli trattati in questo e-book, ma esistono ovviamente molti altri modelli e procedure che consentono di creare app cloud con successo. Il [capitolo finale](more-patterns-and-guidance.md) fornisce i collegamenti alle risorse per gli argomenti che non sono stati trattati in questi 13 modelli.

<a id="resources"></a>
## <a name="resources"></a>Risorse

Per ulteriori informazioni sulle code, vedere le risorse seguenti.

Documentazione:

- [Archiviazione di Microsoft Azure code parte 1: introduzione](https://www.red-gate.com/simple-talk/cloud/platform-as-a-service/microsoft-azure-storage-queues-part-1-getting-started/). Articolo di Roman Schacherl.
- [Esecuzione di attività in background](https://msdn.microsoft.com/library/ff803365.aspx), capitolo 5 del [passaggio di applicazioni nel cloud, terza edizione](https://msdn.microsoft.com/library/ff728592.aspx) da modelli e procedure Microsoft. In particolare, la sezione ["uso delle code di archiviazione di Azure"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- [Procedure consigliate per ottimizzare la scalabilità e la convenienza delle soluzioni di messaggistica basata su coda in Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). White paper di Valery Mizonov.
- [Confronto tra le code di Azure e le code del bus di servizio](https://msdn.microsoft.com/magazine/jj159884.aspx). Nell'articolo di MSDN Magazine sono disponibili informazioni aggiuntive che consentono di scegliere il servizio di Accodamento da utilizzare. L'articolo indica che il bus di servizio dipende da ACS per l'autenticazione, il che significa che le code SB non saranno disponibili quando ACS non è disponibile. Tuttavia, poiché l'articolo è stato scritto, SB è stato modificato per consentire l'uso di [token SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) come alternativa a ACS.
- [Modelli e procedure Microsoft: informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere Nozioni di fondo sulla messaggistica asincrona, modello di pipe e filtri, modello di transazione di compensazione, modello di consumer concorrente, modello CQRS.
- [Percorso CQRS](https://msdn.microsoft.com/library/jj554200). E-book su CQRS by Microsoft Patterns and Practices.

Video:

- [Failsafe: compilazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie di video in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Presenta concetti di alto livello e principi architetturali in modo molto accessibile e interessante, con storie tratte dall'esperienza del team di consulenza clienti Microsoft (CAT) con i clienti effettivi. Per un'introduzione al servizio di archiviazione di Azure e alle code, vedere Episode 5 a partire da 35:13.

> [!div class="step-by-step"]
> [Precedente](distributed-caching.md)
> [Successivo](more-patterns-and-guidance.md)
