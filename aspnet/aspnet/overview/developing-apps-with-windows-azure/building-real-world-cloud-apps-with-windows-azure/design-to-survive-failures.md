---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Progettare per sopravvivere agli errori (compilazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 348232af531b5d53dc3cb46d6d2c7931d95a572d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617726"
---
# <a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Progettazione per sopravvivere agli errori (creazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Uno degli aspetti da considerare quando si compila qualsiasi tipo di applicazione, ma soprattutto uno che verrà eseguito nel cloud in cui verrà usato da molti utenti, è come progettare l'applicazione in modo che possa gestire correttamente gli errori e continuare a fornire valore per quanto riguarda possibile. Dato un tempo sufficiente, si verificherà un errore in qualsiasi ambiente o qualsiasi sistema software. Il modo in cui l'app gestisce tali situazioni determina il modo in cui i clienti otterranno e il tempo necessario per l'analisi e la risoluzione dei problemi.

## <a name="types-of-failures"></a>Tipi di errori

Esistono due categorie di errori di base che si desidera gestire in modo diverso:

- Errori temporanei di correzione automatica, ad esempio problemi di connettività di rete intermittenti.
- Si verificano errori che richiedono un intervento.

Per gli errori temporanei, è possibile implementare un criterio di ripetizione per assicurarsi che la maggior parte del tempo l'app venga recuperata in modo rapido e automatico. I clienti potrebbero notare tempi di risposta leggermente più lunghi, ma in caso contrario non saranno interessati. Verranno illustrati alcuni modi per gestire questi errori nel capitolo relativo alla [gestione degli errori temporanei](transient-fault-handling.md).

In caso di errori, è possibile implementare la funzionalità di monitoraggio e registrazione che invia una notifica tempestiva quando si verificano problemi e che facilita l'analisi della causa radice. Verranno illustrati alcuni modi per aiutare a rimanere oltre questi tipi di errori nel [capitolo monitoraggio e telemetria](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Ambito errore

È anche necessario considerare l'ambito dell'errore, ovvero se un singolo computer è interessato, un intero servizio, ad esempio il database SQL o l'archiviazione, oppure un'intera area.

![Ambito errore](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Errori del computer

In Azure, un server con errore viene sostituito automaticamente da uno nuovo e un'app Cloud ben progettata viene recuperata da questo tipo di errore in modo automatico e rapido. In precedenza abbiamo sottolineato i vantaggi di scalabilità di un livello Web senza stato e la facilità di ripristino da un server in cui si è verificato un errore è un altro vantaggio di questo La facilità di ripristino è anche uno dei vantaggi delle funzionalità di piattaforma distribuita come servizio (PaaS), ad esempio il database SQL e le app Web del servizio app Azure. Gli errori hardware sono rari, ma quando si verificano questi servizi li gestiscono automaticamente. non è nemmeno necessario scrivere codice per gestire gli errori del computer quando si usa uno di questi servizi.

### <a name="service-failures"></a>Errori del servizio

Le app Cloud usano in genere più servizi. Ad esempio, l'app Correggi it usa il servizio di database SQL, il servizio di archiviazione e l'app Web viene distribuita nel servizio app Azure. Quali sono le operazioni eseguite dall'app se uno dei servizi da cui si dipende non riesce? Per alcuni errori del servizio un messaggio descrittivo "dispiace, riprovare più tardi" potrebbe essere il migliore possibile. In molti scenari, tuttavia, è possibile migliorare. Ad esempio, quando l'archivio dati back-end non è attivo, è possibile accettare l'input dell'utente, visualizzare "la richiesta è stata ricevuta" e archiviare l'input in un altro luogo temporaneamente; Quando il servizio necessario è di nuovo operativo, è possibile recuperare l'input ed elaborarlo.

Il capitolo del [modello di lavoro incentrato sulle code](queue-centric-work-pattern.md) Mostra un modo per gestire questo scenario. Il Fix it app archivia le attività nel database SQL, ma non è necessario smettere di funzionare quando il database SQL non è attivo. In questo capitolo verrà illustrato come archiviare l'input dell'utente per un'attività in una coda e come usare un processo di lavoro per leggere la coda e aggiornare l'attività. Se SQL è inattivo, la possibilità di creare correzioni per le attività it non è interessata. il processo di lavoro può attendere ed elaborare nuove attività quando il database SQL è disponibile.

### <a name="region-failures"></a>Errori di area

Le aree intere potrebbero non riuscire. Un'emergenza naturale potrebbe eliminare una data center, potrebbe essere resa flat da un meteorite, la linea di tronco nel Data Center può essere tagliata da un coltivatore che seppellisce una mucca con una terna e così via. Se l'app è ospitata nel Data Center colpito, cosa si può fare? È possibile configurare l'app in Azure per l'esecuzione simultanea in più aree, in modo che, se si verifica una situazione di emergenza, si continui a eseguire in un'altra area. Tali errori sono occorrenze estremamente rare e la maggior parte delle app non attraversa i cerchi necessari per garantire il servizio ininterrotto attraverso errori di questo tipo. Vedere la sezione relativa alle risorse alla fine del capitolo per informazioni su come rendere disponibile l'app anche in caso di errore di un'area.

Uno degli obiettivi di Azure consiste nel rendere molto più semplice la gestione di tutti questi tipi di errori. verranno illustrati alcuni esempi del modo in cui si esegue questa operazione nei capitoli seguenti.

## <a name="slas"></a>Contratti di servizio

Spesso i contratti di servizio (SLA) sono in ascolto nell'ambiente cloud. Fondamentalmente, queste sono le promesse che le aziende rendono affidabili per quanto riguarda il servizio. Un contratto di servizio del 99,9% indica che il servizio deve funzionare correttamente 99,9% del tempo. Si tratta di un valore piuttosto comune per un contratto di contratto e sembra un numero molto elevato, ma è possibile che non si renda conto della quantità di tempo di inattività del 1%. Di seguito è riportata una tabella che mostra il tempo di inattività di diverse percentuali di SLA rispetto a un anno, un mese e una settimana.

![Tabella SLA](design-to-survive-failures/_static/image2.png)

Quindi, un contratto di servizio del 99,9% significa che il servizio potrebbe essere inattivo 8,76 ore all'anno o 43,2 minuti al mese. Si tratta di un tempo di inattività maggiore rispetto alla maggior parte degli utenti. Quindi, gli sviluppatori desiderano tenere presente che è possibile una certa quantità di tempo di inattività e gestirla in modo normale. A un certo punto, qualcuno utilizzerà l'app e un servizio sarà inattivo e si desidera ridurre al minimo l'impatto negativo sul cliente.

Una cosa che è necessario conoscere in merito a un contratto di contratto è la tempistica a cui fa riferimento: l'orologio viene reimpostato ogni settimana, ogni mese o ogni anno? In Azure il clock viene reimpostato ogni mese, il che è migliore per l'utente rispetto a un contratto di licenza annuale, perché un contratto di licenza annuale può nascondere i mesi negativi compensando questi ultimi con una serie di mesi validi.

Naturalmente, aspiriamo sempre meglio rispetto al contratto di contratto; in genere si disporrà di un numero inferiore. Il suggerimento è che se il tempo di inattività è superiore al limite massimo consentito, è possibile richiedere denaro. La quantità di denaro che si ottiene probabilmente non comporterebbe in modo completo l'utente per l'effetto aziendale del tempo di inattività eccessivo, ma tale aspetto del contratto di lavoro funge da criterio di imposizione e informa che l'operazione viene eseguita molto seriamente.

### <a name="composite-slas"></a>Contratti di servizio compositi

Un aspetto importante da considerare quando si esaminano i contratti di servizio è l'effetto dell'uso di più servizi in un'app, con ogni servizio con un contratto di servizio separato. Ad esempio, l'app Correggi it usa app Azure app Web del servizio, archiviazione di Azure e il database SQL. Di seguito sono riportati i numeri di contratto di contratto alla data in cui questo e-book viene scritto nel dicembre 2013:

![Sito Web SLA, archiviazione, database SQL](design-to-survive-failures/_static/image3.png)

Qual è il tempo di inattività massimo che ci si aspetta per l'app in base a questi contratti di servizio? Si potrebbe pensare che il tempo di inattività sarebbe uguale alla percentuale di SLA peggiore o al 99,9% in questo caso. Questo sarebbe vero se tutti e tre i servizi hanno sempre avuto esito negativo nello stesso momento, ma ciò non è necessariamente quello che accade effettivamente. Ogni servizio può non riuscire in modo indipendente in momenti diversi, quindi è necessario calcolare il contratto di servizio composito moltiplicando i singoli numeri di contratto di servizio.

![Contratto di servizio composito](design-to-survive-failures/_static/image4.png)

Quindi, l'app potrebbe essere inattiva non solo 43,2 minuti al mese, ma 3 volte tale quantità, 108 minuti al mese e ancora entro i limiti del contratto di lavoro di Azure.

Questo problema non è univoco in Azure. In realtà, offriamo i migliori contratti di servizio cloud per qualsiasi servizio cloud disponibile e avrai problemi analoghi da affrontare se usi i servizi cloud di qualsiasi fornitore. Ciò che evidenzia è l'importanza di considerare come progettare l'applicazione per gestire gli errori inevitabili del servizio, in quanto potrebbero verificarsi abbastanza spesso da influito sui clienti o sugli utenti.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Contratti di contratto con il cloud rispetto all'esperienza del tempo di inattività aziendale

Le persone a volte dicono "nell'app aziendale non ho mai questi problemi". Se si chiede la quantità di tempo di inattività al mese in cui si trova, in genere si afferma che "si verifica occasionalmente". E se viene chiesto con quale frequenza, ammettono che "a volte è necessario eseguire il backup o installare un nuovo server o aggiornare il software". Ovviamente, questo viene conteggiato come tempo di inattività. La maggior parte delle app aziendali, a meno che non siano particolarmente cruciali, sono in realtà inattive per un periodo di tempo superiore a quello consentito dai contratti di servizio. Tuttavia, quando si tratta di un server e di una propria infrastruttura e si è responsabili dell'IT e del relativo controllo, si tende a provare a ridurre i tempi di inattività. In un ambiente cloud si dipende da un altro utente e non si sa cosa accade, quindi è possibile che si preferisca preoccuparsi.

Quando un'azienda raggiunge una percentuale di tempo di attività maggiore rispetto a un contratto di contratto per il cloud, lo esegue spendendo molto più denaro sull'hardware. Un servizio cloud può eseguire questa operazione, ma dovrebbe addebitare molto altro per i servizi. Al contrario, si sfrutta un servizio economico e si progetta il software in modo che gli errori inevitabili provochino un'interferenza minima per i clienti. Il processo come una finestra di progettazione di app Cloud non è così tanto per evitare errori come per evitare catastrofi e a tale scopo si concentra sul software, non sull'hardware. Mentre le app aziendali si impegnano a massimizzare il tempo medio tra gli errori, le app Cloud si impegnano a ridurre al minimo il tempo medio per il ripristino.

### <a name="not-all-cloud-services-have-slas"></a>Non tutti i servizi cloud hanno contratti di servizio

Tenere presente anche che non tutti i servizi cloud hanno anche un contratto di servizio. Se l'app dipende da un servizio senza una garanzia di tempo di attività, è possibile che si verifichino tempi di inattività molto più lunghi di quanto si possa immaginare. Ad esempio, se si Abilita l'accesso al sito usando un provider di social networking, ad esempio Facebook o Twitter, rivolgersi al provider di servizi per verificare se è presente un contratto di servizio e potrebbe non essere disponibile. Tuttavia, se il servizio di autenticazione diventa inattivo o non è in grado di supportare il volume di richieste che vengono generate, i clienti vengono bloccati dall'app. È possibile che l'utente sia inattivo per giorni o più. Gli autori di una nuova app hanno previsto centinaia di milioni di download e hanno preso una dipendenza dall'autenticazione di Facebook, ma non hanno parlato con Facebook prima di andare in diretta e hanno scoperto che non era previsto alcun contratto di servizio.

### <a name="not-all-downtime-counts-toward-slas"></a>Non tutti i tempi di inattività vengono conteggiati nei contratti

È possibile che alcuni servizi cloud neghino intenzionalmente il servizio se la propria app li utilizza. Questa operazione viene definita *limitazione*. Se un servizio dispone di un contratto di servizio, deve indicare le condizioni in base alle quali è possibile che sia stato limitato e la progettazione dell'app deve evitare tali condizioni e rispondere in modo appropriato alla limitazione, se si verifica. Se, ad esempio, le richieste a un servizio iniziano ad avere esito negativo quando si supera un determinato numero al secondo, si desidera assicurarsi che i tentativi automatici non si verifichino così velocemente che la limitazione continui. Il capitolo relativo alla [gestione degli errori temporanei](transient-fault-handling.md)sarà più approfondito per quanto riguarda la limitazione delle richieste.

## <a name="summary"></a>Riepilogo

Questo capitolo ha provato a comprendere il motivo per cui un'app cloud reale deve essere progettata in modo da evitare gli errori normalmente. A partire dal [capitolo successivo](monitoring-and-telemetry.md), i modelli rimanenti di questa serie vengono illustrati in modo più dettagliato sulle strategie che è possibile usare per eseguire questa operazione:

- Disporre di un [monitoraggio e di una telemetria](monitoring-and-telemetry.md)corretti, in modo da individuare rapidamente gli errori che richiedono l'intervento e disporre di informazioni sufficienti per risolverli.
- [Gestire gli errori temporanei](transient-fault-handling.md) implementando la logica di ripetizione dei tentativi intelligente, in modo che l'app venga ripristinata automaticamente quando è possibile ed esegue il [fallback alla logica di un interruttore.](transient-fault-handling.md#circuitbreakers)
- Utilizzare la [memorizzazione nella cache distribuita](distributed-caching.md) allo scopo di ridurre al minimo i problemi di velocità effettiva, latenza e connessione con l'accesso al database.
- Implementare l'accoppiamento libero tramite il [modello di lavoro incentrato sulla coda](queue-centric-work-pattern.md), in modo che il front-end dell'app possa continuare a funzionare quando il back-end è inattivo.

## <a name="resources"></a>Risorse

Per ulteriori informazioni, vedere i capitoli successivi in questo e-book e le risorse seguenti.

Documentazione:

- [Failsafe: linee guida per architetture cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper di Marc Mercuri, Ulrich Homann e Andrew Townhill. Versione di pagina Web della serie di video FailSafe.
- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi cloud di Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper di Mark Simms e Michael Thomassy.
- [Informazioni tecniche sulla continuità aziendale di Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). White paper di Patrick Wickline e Jason Roth.
- [Ripristino di emergenza e disponibilità elevata per le applicazioni Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). White paper di Michael McKeown, Hanu Kommalapati e Jason Roth.
- [Modelli e procedure Microsoft: informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere linee guida per la distribuzione di più data center, modello di interruttore.
- [Supporto di Azure-contratti di servizio](https://azure.microsoft.com/support/legal/sla/).
- [Continuità aziendale nel database SQL di Azure](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Documentazione sulle funzionalità di disponibilità elevata e ripristino di emergenza del database SQL.
- [Disponibilità elevata e ripristino di emergenza per SQL Server in macchine virtuali di Azure](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Video

- [Failsafe: compilazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Presenta concetti di alto livello e principi architetturali in modo molto accessibile e interessante, con storie tratte dall'esperienza del team di consulenza clienti Microsoft (CAT) con i clienti effettivi. Gli episodi 1 e 8 approfondiscono i motivi della progettazione delle app cloud per sopravvivere agli errori. Vedere anche la discussione successiva sulla limitazione nell'episodio 2 a partire da 49:57, la discussione dei punti di errore e delle modalità di errore nell'episodio 2 a partire da 56:05 e la discussione degli interruttori in Episode 3 a partire da 40:55.
- [Creazione di grandi dimensioni: lezioni apprese dai clienti di Azure-parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms parla di progettazione per l'errore e la strumentazione di tutto. In modo analogo alla serie failsafe, ma passa ad altre procedure dettagliate.

> [!div class="step-by-step"]
> [Precedente](unstructured-blob-storage.md)
> [Successivo](monitoring-and-telemetry.md)
