---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opzioni di archiviazione dei dati (compilazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: f97d973d87db895441f813376d757a8a2e94b255
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585935"
---
# <a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opzioni di archiviazione dei dati (compilazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

La maggior parte degli utenti viene usata per i database relazionali e tende a trascurare altre opzioni di archiviazione dei dati durante la progettazione di un'app cloud. Il risultato può essere prestazioni non ottimali, costi elevati o peggiori, perché i database [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (non relazionali) possono gestire alcune attività in modo più efficiente rispetto ai database relazionali. Quando i clienti chiedono assistenza per la risoluzione di un problema critico di archiviazione dei dati, spesso si tratta di un database relazionale in cui una delle opzioni NoSQL avrebbe funzionato meglio. In queste situazioni il cliente sarebbe stato migliore se avesse implementato la soluzione NoSQL prima di distribuire l'app nell'ambiente di produzione.

D'altra parte, sarebbe anche un errore presumere che un database NoSQL possa eseguire tutte le operazioni in modo corretto o sufficiente. Non esiste una singola scelta ottimale per la gestione dei dati per tutte le attività di archiviazione dei dati; soluzioni di gestione dati diverse sono ottimizzate per diverse attività. La maggior parte delle app Cloud reali presenta una serie di requisiti di archiviazione dei dati e vengono spesso serviti con una combinazione di più soluzioni di archiviazione dati.

Lo scopo di questo capitolo è fornire un senso più ampio delle opzioni di archiviazione dei dati disponibili per un'app cloud e alcune indicazioni di base su come scegliere quelle adatte allo scenario. È preferibile tenere presente le opzioni disponibili e valutare i loro punti di forza e debolezza prima di sviluppare un'applicazione. Modificare le opzioni di archiviazione dei dati in un'app di produzione può essere estremamente difficile, ad esempio la necessità di modificare un motore Jet mentre il piano è in corso.

## <a name="data-storage-options-on-azure"></a>Opzioni di archiviazione dati in Azure

Il cloud rende relativamente semplice l'uso di un'ampia gamma di archivi dati relazionali e NoSQL. Di seguito sono riportate alcune delle piattaforme di archiviazione dati che è possibile usare in Azure.

![](data-storage-options/_static/image1.png)

La tabella mostra quattro tipi di database NoSQL:

- I [database chiave/valore](https://msdn.microsoft.com/library/dn313285.aspx#sec7) archiviano un singolo oggetto serializzato per ogni valore di chiave. Sono utili per l'archiviazione di grandi volumi di dati in cui si vuole ottenere un elemento per un determinato valore di chiave e non è necessario eseguire query in base ad altre proprietà dell'elemento.

    [Archiviazione BLOB di Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) è un database chiave/valore che funziona come l'archiviazione di file nel cloud, con valori di chiave corrispondenti ai nomi di file e cartelle. È possibile recuperare un file in base alla cartella e al nome file, non cercando i valori nel contenuto del file.

    L' [archiviazione tabelle di Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) è anche un database chiave/valore. Ogni valore viene chiamato *entità* (simile a una riga, identificata da una chiave di partizione e da una chiave di riga) e contiene più *Proprietà* (simili alle colonne, ma non tutte le entità di una tabella devono condividere le stesse colonne). L'esecuzione di query su colonne diverse dalla chiave è estremamente inefficiente e deve essere evitata. Ad esempio, è possibile archiviare i dati del profilo utente, con una partizione che archivia le informazioni relative a un singolo utente. È possibile archiviare i dati, ad esempio nome utente, hash password, data di nascita e così via, in proprietà separate di un'entità o in entità separate nella stessa partizione. Ma non si vuole eseguire una query per tutti gli utenti con un determinato intervallo di date di nascita e non è possibile eseguire una query di join tra la tabella del profilo e un'altra tabella. L'archiviazione tabelle è più scalabile e meno costosa rispetto a un database relazionale, ma non Abilita query o join complessi.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) sono database chiave/valore in cui i valori sono *documenti*. "Document" non viene usato nel senso di un documento di Word o di Excel, ma indica una raccolta di campi e valori denominati, ognuno dei quali può essere un documento figlio. Ad esempio, in una tabella di cronologia degli ordini un documento di ordine può includere i campi Numero ordine, Data ordine e cliente; e il campo Customer potrebbe avere campi nome e indirizzo. Il database codifica i dati del campo in un formato, ad esempio XML, YAML, JSON o BSON; oppure può utilizzare testo normale. Una funzionalità che imposta i database di documenti oltre ai database chiave/valore è la possibilità di eseguire query su campi non chiave e definire indici secondari per rendere più efficiente l'esecuzione di query. Questa possibilità rende un database di documenti più adatto per le applicazioni che devono recuperare i dati in base a criteri più complessi rispetto al valore della chiave del documento. Ad esempio, in un database di documenti di cronologia ordini di vendita è possibile eseguire una query su diversi campi, ad esempio ID prodotto, ID cliente, nome del cliente e così via. [MongoDB](http://www.mongodb.org/) è un database di documenti noto.
- I [database a colonne](https://msdn.microsoft.com/library/dn313285.aspx#sec9) sono archivi dati chiave/valore che consentono di strutturare l'archiviazione dei dati in raccolte di colonne correlate denominate colonne di gruppo. Ad esempio, un database Census potrebbe avere un gruppo di colonne per il nome di una persona (First, Middle, Last), un gruppo per l'indirizzo della persona e un gruppo per le informazioni sul profilo della persona (DOB, Gender e così via). Il database può quindi archiviare ogni famiglia di colonne in una partizione separata mantenendo tutti i dati per una persona correlata alla stessa chiave. È quindi possibile leggere tutte le informazioni sul profilo senza dover leggere anche tutte le informazioni sul nome e sull'indirizzo. [Cassandra](http://cassandra.apache.org/) è un noto database a colonne.
- I [database Graph](https://msdn.microsoft.com/library/dn313285.aspx#sec10) archiviano le informazioni come una raccolta di oggetti e relazioni. Lo scopo di un database Graph consiste nel consentire a un'applicazione di eseguire in modo efficiente le query che attraversano la rete di oggetti e le relazioni tra di essi. Ad esempio, gli oggetti potrebbero essere dipendenti in un database delle risorse umane e potrebbe essere necessario facilitare le query, ad esempio "Trova tutti i dipendenti che lavorano direttamente o indirettamente per Scott". [Neo4j](http://www.neo4j.org/) è un noto database Graph.

Rispetto ai database relazionali, le opzioni NoSQL offrono scalabilità e convenienza molto più elevate per l'archiviazione e l'analisi di dati non strutturati. Il compromesso è che non forniscono le funzionalità avanzate di query e di integrità dei dati dei database relazionali. NoSQL funziona bene per i dati di log di IIS, che includono un volume elevato senza necessità di eseguire query join. NoSQL non funzionerà così bene per le transazioni bancarie, che richiedono l'integrità assoluta dei dati e implicano molte relazioni con altri dati correlati agli account.

È inoltre disponibile una categoria più recente di piattaforma di database denominata [NewSQL](http://en.wikipedia.org/wiki/NewSQL) che combina la scalabilità di un database di NoSQL con la query e l'integrità transazionale di un database relazionale. I database NewSQL sono progettati per l'elaborazione delle query e dell'archiviazione distribuita, che è spesso difficile da implementare nei database "OldSQL". [NuoDB](http://www.nuodb.com/) è un esempio di database NewSQL che può essere usato in Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop e MapReduce

I volumi elevati di dati che è possibile archiviare nei database NoSQL possono risultare difficili da analizzare in modo tempestivo. A tale scopo, è possibile usare un Framework come [Hadoop](http://hadoop.apache.org/) che implementa la funzionalità [MapReduce](http://en.wikipedia.org/wiki/MapReduce) . In sostanza, il processo MapReduce è il seguente:

- Limitare le dimensioni dei dati che devono essere elaborati selezionando l'archivio dati solo i dati effettivamente necessari per l'analisi. Si desidera, ad esempio, che si conosca il trucco della base utente per anno di nascita, quindi si selezionano solo gli anni di nascita dall'archivio dati del profilo utente.
- Suddividere i dati in parti e inviarli a computer diversi per l'elaborazione. Il computer A calcola il numero di persone con 1950-1959 di date, il computer B esegue 1960-1969 e così via. Questo gruppo di computer è denominato *cluster Hadoop*.
- Inserire di nuovo i risultati di ogni parte dopo che l'elaborazione delle parti è stata eseguita. È ora disponibile un elenco relativamente breve del numero di persone per ogni anno di nascita e l'attività di calcolo delle percentuali in questo elenco generale è gestibile.

In Azure [HDInsight](https://azure.microsoft.com/services/hdinsight/) consente di elaborare, analizzare e ottenere nuove informazioni da Big data usando le potenzialità di Hadoop. Ad esempio, è possibile usarlo per analizzare i log del server Web:

- Abilitare la registrazione del server Web nell'account di archiviazione. In questo modo viene impostato Azure per scrivere i log nel servizio BLOB per ogni richiesta HTTP all'applicazione. Il servizio BLOB è fondamentalmente l'archiviazione di file cloud e si integra bene con HDInsight.

    ![Log nell'archivio BLOB](data-storage-options/_static/image2.png)
- Quando l'app ottiene il traffico, i log IIS del server Web vengono scritti nell'archivio BLOB.

    ![Log del server Web](data-storage-options/_static/image3.png)
- Nel portale fare clic su **nuovo** - **servizi dati** - **HDInsight** - **creazione rapida**e specificare il nome del cluster HDInsight, le dimensioni del cluster (numero di nodi dati del cluster HDInsight) e un nome utente e una password per il cluster HDInsight.

    ![HDInsight](data-storage-options/_static/image4.png)

È ora possibile configurare i processi di MapReduce per analizzare i log e ottenere risposte a domande come:

- Quali sono le ore del giorno in cui l'app ottiene il traffico più o meno?
- Quali paesi provengono dal traffico?
- Qual è il reddito medio del quartiere delle aree da cui deriva il traffico. (È disponibile un set di dati pubblico che fornisce il reddito del quartiere in base all'indirizzo IP ed è possibile trovare la corrispondenza con l'indirizzo IP nei log del server Web.)
- In che modo il reddito di quartiere è correlato a pagine o prodotti specifici nel sito?

È quindi possibile usare le risposte a domande come queste per la destinazione degli annunci in base alla probabilità che un cliente sia interessato o possa acquistare un determinato prodotto.

Come illustrato nel capitolo automatizzare [tutto](automate-everything.md), la maggior parte delle funzioni che è possibile eseguire nel portale può essere automatizzata e che include la configurazione e l'esecuzione di processi di analisi di HDInsight. Uno script HDInsight tipico potrebbe contenere i passaggi seguenti:

- Effettuare il provisioning di un cluster HDInsight e collegarlo all'account di archiviazione per l'input dell'archiviazione BLOB.
- Caricare i file eseguibili del processo MapReduce (file con estensione jar o exe) nel cluster HDInsight.
- Inviare un MapReduce che archivia i dati di output nell'archivio BLOB.
- Attendere il completamento del processo.
- Eliminare il cluster HDInsight.
- Accedere all'output dall'archiviazione BLOB.

Eseguendo uno script che esegue questa operazione, è possibile ridurre al minimo la quantità di tempo per cui viene effettuato il provisioning del cluster HDInsight, riducendo al minimo i costi.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Piattaforma distribuita come servizio (PaaS) rispetto all'infrastruttura distribuita come servizio (IaaS)

Le opzioni di archiviazione dei dati elencate in precedenza includono soluzioni PaaS (Platform-as-a-Service) e IaaS (Infrastructure-as-a-Service). PaaS consente di gestire l'infrastruttura hardware e software e di utilizzare semplicemente il servizio. Il database SQL è una funzionalità PaaS di Azure. È possibile richiedere i database e, in background, Azure imposta e configura le VM e imposta i database su di essi. Non è possibile accedere direttamente alle macchine virtuali e non è necessario gestirle. IaaS significa che si configurano, configurano e gestiscono le macchine virtuali eseguite nell'infrastruttura data center e si inseriscono tutti gli elementi desiderati. Viene fornita una raccolta di immagini di VM preconfigurate per le configurazioni di VM comuni. Ad esempio, è possibile installare immagini di VM preconfigurate per Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle Database e così via.

Le soluzioni di dati PaaS offerte da Azure includono:

- Database SQL di Azure (precedentemente noto come SQL Azure). Un database relazionale cloud basato su SQL Server.
- Archiviazione tabelle di Azure. Un database NoSQL chiave/valore.
- Archiviazione BLOB di Azure. Archiviazione file nel cloud.

Per IaaS, è possibile eseguire qualsiasi operazione che è possibile caricare in una macchina virtuale, ad esempio:

- Database relazionali, ad esempio SQL Server, Oracle, MySQL, SQL Compact, SQLite o postgres.
- Archivi dati chiave/valore, ad esempio memcached, Redis, Cassandra e Riak.
- Archivi dati della colonna, ad esempio HBase.
- Database di documenti quali MongoDB, RavenDB e CouchDB.
- Database Graph, ad esempio Neo4j.

![Opzioni di archiviazione dati in Azure](data-storage-options/_static/image5.png)

L'opzione IaaS offre opzioni di archiviazione dei dati quasi illimitate e molte di esse sono particolarmente facili da usare perché è possibile creare macchine virtuali usando immagini preconfigurate. Ad esempio, nel portale di gestione passare a **macchine virtuali**, fare clic sulla scheda **Immagini** e fare clic su **Esplora VM Depot**.

![Esplora VM Depot](data-storage-options/_static/image6.png)

Viene quindi visualizzato un elenco di [centinaia di immagini di VM preconfigurate](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx)ed è possibile creare una macchina virtuale da un'immagine con un sistema di gestione di database preinstallato, ad esempio MongoDB, Neo4J, Redis, Cassandra o CouchDB:

![MongoDB in VM Depot](data-storage-options/_static/image7.png)

Azure rende le opzioni di archiviazione dei dati di IaaS più facili da usare possibile, ma le offerte PaaS presentano molti vantaggi che li rendono più convenienti e pratici per molti scenari:

- Non è necessario creare macchine virtuali, ma è sufficiente usare il portale o uno script per configurare un archivio dati. Se si vuole un archivio dati da 200 terabyte, è possibile fare semplicemente clic su un pulsante o eseguire un comando e, in secondi, è pronto per l'uso.
- Non è necessario gestire o applicare patch alle macchine virtuali usate dal servizio; Microsoft esegue questa operazione automaticamente.-non è necessario preoccuparsi della configurazione dell'infrastruttura per la scalabilità o la disponibilità elevata; Microsoft gestisce tutto automaticamente.
- Non è necessario acquistare le licenze; i costi di licenza sono inclusi nei costi del servizio.
- Paghi solo per ciò che usi.

Le opzioni di archiviazione dei dati di PaaS in Azure includono offerte da provider di terze parti. Ad esempio, è possibile scegliere il [componente aggiuntivo MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) da Azure Store per eseguire il provisioning di un database MongoDB come servizio.

## <a name="choosing-a-data-storage-option"></a>Scelta di un'opzione di archiviazione dati

Non esiste un approccio adatto a tutti gli scenari. Se qualcuno afferma che questa tecnologia è la risposta, la prima cosa da fare è "Qual è la domanda?", perché diverse soluzioni sono ottimizzate per diversi aspetti. I vantaggi del modello relazionale sono evidenti. Questo è il motivo per cui è stato così lungo. Tuttavia, sono disponibili anche i lati inattivi di SQL che possono essere risolti con una soluzione NoSQL.

Spesso ciò che si nota meglio è un approccio composizionale, in cui si usano SQL e NoSQL in un'unica soluzione. Anche quando le persone dicono di adottare NoSQL, se si analizzano le attività che stanno facendo, spesso si scopre che usano diversi framework NoSQL: usano [CouchDB](http://wiki.apache.org/couchdb/Introduction), [Redis](http://redis.io/)e [Riak](http://basho.com/riak/) per diversi aspetti. Anche Facebook, che usa NoSQL in modo estensivo, USA diversi framework NoSQL per diverse parti del servizio. La flessibilità di combinare e abbinare gli approcci di archiviazione dei dati è uno degli aspetti più interessanti del cloud, poiché è facile usare più soluzioni di dati e integrarle in un'unica app.

Di seguito sono riportate alcune domande da considerare quando si sceglie un approccio:

| Semantica dei dati | -Qual è la semantica di archiviazione dei dati e di accesso ai dati principale (si archiviano dati relazionali o non strutturati)? I dati non strutturati, ad esempio i file multimediali, si adattano meglio all'archiviazione BLOB. una raccolta di dati correlati, ad esempio prodotti, inventari, fornitori, ordini dei clienti e così via, si adatta meglio a un database relazionale. |
| --- | --- |
| Supporto delle query | -Quanto è semplice eseguire una query sui dati? -Quali tipi di domande possono essere poste in modo efficiente? Gli archivi dati chiave/valore sono molto utili per ottenere una singola riga, dato un valore di chiave, ma non così efficace per le query complesse. Per un archivio dati profilo utente in cui si ricevono sempre i dati per un utente specifico, un archivio dati chiave/valore potrebbe funzionare correttamente; per un catalogo di prodotti in cui si desidera ottenere diversi raggruppamenti basati su diversi attributi del prodotto, un database relazionale potrebbe funzionare meglio. I database NoSQL possono archiviare in modo efficiente volumi elevati di dati, ma è necessario strutturare il database in base al modo in cui l'app esegue query sui dati, rendendo le query ad hoc più difficili da eseguire. Con un database relazionale è possibile compilare praticamente qualsiasi tipo di query. |
| Proiezione funzionale | -Sul lato server è possibile eseguire domande, aggregazioni e così via? Se eseguo SELECT COUNT (\*) da una tabella in SQL, il lavoro viene eseguito in modo molto efficiente sul server e viene restituito il numero che sto cercando. Se si vuole lo stesso calcolo da un archivio dati NoSQL che non supporta l'aggregazione, si tratta di una query non vincolata inefficiente e probabilmente si verifichi un timeout. Anche se la query ha esito positivo, è necessario recuperare tutti i dati dal server al client e contare le righe nel client. -Quali linguaggi o tipi di espressioni possono essere usati? Con un database relazionale posso usare SQL. Con alcuni database NoSQL, ad esempio l'archiviazione tabelle di Azure, utilizzerò [OData](http://www.odata.org/)e posso solo filtrare la chiave primaria e ottenere le proiezioni (selezionare un subset dei campi disponibili). |
| Facilità di scalabilità | -Con quale frequenza e quanto è necessario ridimensionare i dati? -La piattaforma implementa in modo nativo la scalabilità orizzontale? -Quanto è facile aggiungere/rimuovere capacità (dimensioni e velocità effettiva)? I database relazionali e le tabelle non vengono partizionati automaticamente per renderli scalabili, quindi sono difficili da scalare oltre alcune limitazioni. Gli archivi dati NoSQL come l'archiviazione tabelle di Azure intrinsecamente partizionano tutti gli elementi e non esiste quasi nessun limite per l'aggiunta di partizioni. È possibile ridimensionare facilmente l'archiviazione tabelle fino a 200 terabyte, ma le dimensioni massime del database per il database SQL di Azure sono di 500 gigabyte. È possibile ridimensionare i dati relazionali partizionando i dati in più database, ma la configurazione di un'applicazione per il supporto di tale modello comporta una grande quantità di attività di programmazione. |
| Strumentazione e gestibilità | -Quanto è facile la piattaforma per instrumentare, monitorare e gestire? È necessario essere informati sull'integrità e sulle prestazioni dell'archivio dati, pertanto è necessario conoscere in anticipo le metriche che una piattaforma offre gratuitamente e ciò che è necessario sviluppare autonomamente. |
| Operazioni | -Quanto è semplice la piattaforma per la distribuzione e l'esecuzione in Azure? PaaS? IaaS? Linux? L'archiviazione tabelle e il database SQL sono facili da configurare in Azure. Le piattaforme che non sono soluzioni predefinite di Azure PaaS richiedono più impegno. |
| Supporto delle API | -È disponibile un'API che semplifica l'uso della piattaforma? Per il servizio tabelle di Azure è disponibile un SDK con un'API .NET che supporta il modello di programmazione asincrona di .NET 4,5. Se si sta scrivendo un'app .NET, sarà molto più semplice scrivere e testare il codice per il servizio tabelle di Azure rispetto a un'altra piattaforma di archivio dati di colonne chiave/valore senza API o con una meno completa. |
| Integrità delle transazioni e coerenza dei dati | -È fondamentale che la piattaforma supporti le transazioni per garantire la coerenza dei dati? Per tenere traccia dei messaggi di posta elettronica in blocco inviati, le prestazioni e i costi di archiviazione dei dati bassi potrebbero essere più importanti del supporto automatico per le transazioni o l'integrità referenziale nella piattaforma dati, rendendo il servizio tabelle di Azure una scelta ottimale. Per tenere traccia dei saldi del conto bancario o degli ordini di acquisto, una piattaforma di database relazionale che fornisce garanzie transazionali complesse costituisce una scelta migliore. |
| Continuità aziendale | -Quanto è facile eseguire backup, ripristino e ripristino di emergenza? I dati di produzione prima o dopo saranno danneggiati e sarà necessaria una funzione di annullamento. I database relazionali presentano spesso funzionalità di ripristino con granularità fine, ad esempio la possibilità di eseguire il ripristino a un momento specifico. Conoscere le funzionalità di ripristino disponibili in ogni piattaforma che si sta considerando è un fattore importante da considerare. |
| Cost | -Se più piattaforme sono in grado di supportare il carico di lavoro dei dati, come si confrontano i costi? Se ad esempio si usa ASP.NET Identity, è possibile archiviare i dati del profilo utente nel servizio tabelle di Azure o nel database SQL di Azure. Se non sono necessarie le funzionalità di query avanzate del database SQL, è possibile scegliere le tabelle di Azure in parte perché il costo è molto inferiore per una determinata quantità di spazio di archiviazione. |

Ciò che in genere si consiglia è conoscere la risposta alle domande in ognuna di queste categorie prima di scegliere le soluzioni di archiviazione dati.

Inoltre, il carico di lavoro potrebbe avere requisiti specifici che alcune piattaforme possono supportare meglio di altre. Ad esempio:

- L'applicazione richiede funzionalità di controllo?
- Quali sono i requisiti di longevità dei dati: sono necessarie funzionalità di archiviazione o eliminazione automatica?
- Si hanno esigenze di sicurezza specializzate? Ad esempio, i dati includono informazioni personali (informazioni personali), ma è necessario essere in grado di verificare che le informazioni personali siano escluse dai risultati della query.
- Se sono presenti dati che non possono essere archiviati nel cloud per motivi normativi o tecnologici, potrebbe essere necessaria una piattaforma di archiviazione dei dati cloud che facilita l'integrazione con l'archiviazione locale.

## <a name="demo--using-sql-database-in-azure"></a>Demo: uso del database SQL in Azure

L'app Correggi it usa un database relazionale per archiviare le attività. Lo script di Windows PowerShell per la creazione dell'ambiente illustrato nel [capitolo automatizzare tutto](automate-everything.md) crea due istanze del database SQL. È possibile visualizzarli nel portale facendo clic sulla scheda **database SQL** .

![Database SQL nel portale](data-storage-options/_static/image8.png)

È anche facile creare database usando il portale.

Fare clic su **nuovo--Data Services** -- **database SQL** -- **creazione rapida**, immettere un nome di database, scegliere un server già presente nell'account o crearne uno nuovo, quindi fare clic su **Crea database SQL**.

![Nuovo database SQL](data-storage-options/_static/image9.png)

Attendere alcuni secondi e disporre di un database in Azure pronto per l'uso.

![Nuovo database SQL creato](data-storage-options/_static/image10.png)

Quindi, Azure esegue in pochi secondi le operazioni che potrebbero richiedere un giorno o una settimana o più tempo per l'esecuzione nell'ambiente locale. Poiché è possibile creare i database automaticamente in uno script o usando un'API di gestione, è possibile aumentare la scalabilità orizzontale distribuendo i dati in più database, a condizione che l'applicazione sia stata programmata.

Questo è un esempio del modello di piattaforma distribuita come servizio. Non è necessario gestire i server. Non è necessario preoccuparsi dei backup. Viene eseguito in disponibilità elevata: i dati nel database vengono replicati automaticamente in tre server. Se un computer muore, viene eseguito automaticamente il failover e non si perdono i dati. Il server viene regolarmente sottoposta a patch, non è necessario preoccuparsi.

Fare clic su un pulsante per ottenere la stringa di connessione esatta necessaria e iniziare subito a usare il nuovo database.

![Stringhe di connessione](data-storage-options/_static/image11.png)

Il dashboard mostra la cronologia delle connessioni e la quantità di spazio di archiviazione usata.

![Dashboard del database SQL](data-storage-options/_static/image12.png)

È possibile gestire i database nel portale o usando SQL Server gli strumenti con cui si ha già familiarità, inclusi SQL Server Management Studio (SSMS) e Visual Studio Tools Esplora oggetti di SQL Server (SSOX) e Esplora server.

![SSOX](data-storage-options/_static/image13.png)

Un altro aspetto interessante è il modello di determinazione dei prezzi. Puoi iniziare lo sviluppo con un database gratuito di 20 MB e un database di produzione inizia a circa $5 al mese. Si paga solo la quantità di dati effettivamente archiviati nel database, non la capacità massima. Non è necessario acquistare una licenza.

Il database SQL è facile da scalare. Per l'app Correggi it, il database creato nello script di automazione è limitato a 1 Gig. Se si vuole ridimensionarlo fino a 150 Gig, è sufficiente passare al portale e modificare l'impostazione oppure eseguire un comando API REST e, in pochi secondi, un database Gig 150 in cui è possibile distribuire i dati.

![Edizioni e dimensioni del database SQL](data-storage-options/_static/image14.png)

Questa è la potenza del cloud per creare un'infrastruttura in modo rapido e semplice e iniziare subito a usarla.

L'app Correggi it usa due database SQL, uno per l'appartenenza (autenticazione e autorizzazione) e uno per i dati e questo è tutto ciò che serve per eseguire il provisioning e ridimensionarlo. In precedenza è stato illustrato come eseguire il provisioning dei database tramite gli script di Windows PowerShell e ora è stato anche illustrato quanto sia facile eseguire nel portale.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework rispetto all'accesso diretto al database tramite ADO.NET

L'app Correggi it accede a questi database usando il Entity Framework, ORM consigliato da Microsoft (mapper relazionale a oggetti) per le applicazioni .NET. Un ORM è un ottimo strumento che facilita la produttività degli sviluppatori, ma la produttività comporta una riduzione delle prestazioni in alcuni scenari. In un'app cloud reale non è possibile scegliere tra l'uso di EF o l'uso diretto di ADO.NET. si useranno entrambi. Nella maggior parte dei casi, quando si scrive codice che funziona con il database, ottenere le massime prestazioni non è fondamentale ed è possibile sfruttare i vantaggi della codifica e dei test semplificati ottenuti con la Entity Framework. Nelle situazioni in cui il sovraccarico EF provocherebbe prestazioni inaccettabili, è possibile scrivere ed eseguire query personalizzate usando ADO.NET, idealmente chiamando stored procedure.

Indipendentemente dal metodo utilizzato per accedere al database, è necessario ridurre al minimo "chattiness". In altre parole, se è possibile ottenere tutti i dati necessari in un set di risultati della query più grande, anziché dozzine o centinaia di quelli più piccoli, questo è in genere preferibile. Se, ad esempio, è necessario elencare gli studenti e i corsi in cui sono iscritti, è in genere preferibile ottenere tutti i dati in una query di join anziché recuperare gli studenti in una query ed eseguire query separate per i corsi di ogni studente.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Database SQL e Entity Framework nell'app Correggi it

Nell'app Fix it la classe `FixItContext`, che deriva dalla classe Entity Framework `DbContext`, identifica il database e specifica le tabelle nel database. Il contesto specifica un set di entità (tabella) per le attività e il codice passa al contesto il nome della stringa di connessione. Il nome fa riferimento a una stringa di connessione definita nel file Web. config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

La stringa di connessione nel file *Web. config* è denominata appdb, che punta al database di sviluppo locale:

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Il Entity Framework crea una tabella *FixItTasks* in base alle proprietà incluse nella classe `FixItTask` Entity. Si tratta di una semplice classe POCO (Plain Old CLR Object), che significa che non eredita da o che ha dipendenze dalla Entity Framework. Tuttavia Entity Framework sa come creare una tabella basata su di essa ed eseguire operazioni CRUD (create-Read-Update-Delete).

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabella FixItTasks](data-storage-options/_static/image15.png)

L'app Correggi it include un'interfaccia del repository che usa per le operazioni CRUD che utilizzano l'archivio dati.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Si noti che i metodi del repository sono tutti asincroni, quindi l'accesso ai dati può essere eseguito in modo completamente asincrono.

L'implementazione del repository chiama Entity Framework metodi asincroni per lavorare con i dati, incluse le query LINQ, nonché per le operazioni di inserimento, aggiornamento ed eliminazione. Di seguito è riportato un esempio di codice per la ricerca di un'attività Correggi it.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Si noterà che è presente anche un codice di registrazione degli errori e dei tempi, che verrà esaminato più avanti nel [capitolo monitoraggio e telemetria](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Scelta del database SQL (PaaS) rispetto a SQL Server in una VM (IaaS) in Azure

Un aspetto interessante di SQL Server e del database SQL di Azure è che il modello di programmazione principale per entrambi è identico. È possibile utilizzare la maggior parte delle competenze in entrambi gli ambienti. È anche possibile usare un database di SQL Server in fase di sviluppo e un'istanza di database SQL nel cloud, che è il modo in cui è configurata l'app per la correzione.

In alternativa, è possibile eseguire lo stesso SQL Server nel cloud eseguito in locale mediante l'installazione in macchine virtuali IaaS. Per alcune applicazioni legacy, l'esecuzione di SQL Server in una macchina virtuale potrebbe essere una soluzione migliore. Poiché un database di SQL Server viene eseguito in una macchina virtuale dedicata, dispone di più risorse rispetto a un database SQL in esecuzione su un server condiviso. Ciò significa che un database di SQL Server può essere di dimensioni maggiori e comunque funzionare correttamente. In generale, le dimensioni del database e le dimensioni della tabella sono inferiori, migliore sarà il caso d'uso per il database SQL (PaaS).

Di seguito sono riportate alcune linee guida su come scegliere tra i due modelli.

| Database SQL di Azure (PaaS) | SQL Server in una macchina virtuale (IaaS) |
| --- | --- |
| **Vantaggi** : non è necessario creare o gestire macchine virtuali, aggiornare o applicare patch al sistema operativo o a SQL; Questa operazione viene eseguita da Azure. -Disponibilità elevata incorporata, con contratto di contratto a livello di database. -Costo totale di proprietà (TCO) basso perché si paga solo per le risorse usate (nessuna licenza necessaria). -Adatto per la gestione di un numero elevato di database più piccoli (&lt;= 500 GB ciascuno). -È facile creare in modo dinamico nuovi database per abilitare la scalabilità orizzontale. | ***Vantaggi*** : funzionalità compatibile con SQL Server locali. -È in grado di implementare SQL Server [disponibilità elevata tramite AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) in due macchine virtuali con contratto di macchina virtuale a livello di VM. -Si ha il controllo completo sulla modalità di gestione di SQL. -È possibile riutilizzare le licenze SQL di cui si è già proprietari o pagare per un'ora. -Adatto per la gestione di un minor numero di database, ma più grandi (1 TB +). |
| **Svantaggi** : alcuni gap di funzionalità rispetto ai SQL Server locali (mancanza di [integrazione CLR](https://technet.microsoft.com/library/ms131102.aspx), Transparent Data [Encryption](https://technet.microsoft.com/library/bb934049.aspx), [supporto della compressione](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx)e così via)-limite delle dimensioni del database pari a 500 GB. | ***Svantaggi*** : aggiornamenti/patch (sistema operativo e SQL) sono responsabilità della creazione e della gestione dei database: le operazioni di i/o al secondo per il disco sono limitate a circa 8000 (tramite 16 unità dati). |

Se si vuole usare SQL Server in una macchina virtuale, è possibile usare la propria licenza SQL Server oppure è possibile pagare uno per ogni ora. Ad esempio, nel portale o tramite l'API REST è possibile creare una nuova macchina virtuale usando un'immagine SQL Server.

![Creare una macchina virtuale con SQL Server](data-storage-options/_static/image16.png)

![Elenco di immagini di SQL Server VM](data-storage-options/_static/image17.png)

Quando si crea una macchina virtuale con un'immagine di SQL Server, il costo della licenza di SQL Server viene proporzionato per ora in base all'utilizzo della VM. Se si dispone di un progetto che verrà eseguito solo per un paio di mesi, è più economico pagare in base all'ora. Se si ritiene che il progetto durerà per anni, è più conveniente acquistare la licenza nel modo consueto.

## <a name="summary"></a>Riepilogo

Il cloud computing semplifica la combinazione e la corrispondenza degli approcci di archiviazione dei dati per soddisfare al meglio le esigenze dell'applicazione. Se si sta creando una nuova applicazione, è opportuno valutare attentamente le domande qui elencate per scegliere gli approcci che continueranno a funzionare correttamente quando l'applicazione cresce. Il [capitolo successivo](data-partitioning-strategies.md) spiegherà alcune strategie di partizionamento che è possibile usare per combinare più approcci di archiviazione dati.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere le seguenti risorse.

Scelta di una piattaforma di database:

- [Accesso ai dati per soluzioni a scalabilità elevata: uso di SQL, NoSQL e di persistenza poliglotta](https://aka.ms/dag-doc). E-book di Microsoft Patterns and Practices che approfondisce i diversi tipi di archivi dati disponibili per le applicazioni cloud.
- [Modelli e procedure Microsoft: informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/ff898430.aspx). Vedere Nozioni di fondo sulla coerenza dei dati, Guida alla replica e alla sincronizzazione dei dati, modello di tabella dell'indice, modello di vista materializzata.
- [Base: alternativa acid](http://queue.acm.org/detail.cfm?id=1394128). Articolo sui compromessi tra coerenza e scalabilità dei dati.
- [Sette database in sette settimane: una guida per i database moderni e il movimento NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Libro di Eric Redmond e Jim R. Wilson. Si consiglia vivamente di presentarsi all'ampia gamma di piattaforme di archiviazione dati attualmente disponibili.

Scelta tra SQL Server e il database SQL:

- [Informazioni aggiuntive sull'anteprima Premium per il database SQL](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Introduzione a database SQL Premium e informazioni aggiuntive su quando sceglierlo tramite le edizioni Web e business del database SQL.
- [Linee guida e limitazioni (database SQL di Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Pagina del portale che fornisce collegamenti alla documentazione sulle limitazioni del database SQL, inclusa una che è incentrata sulle funzionalità di SQL Server che il database SQL non supporta.
- [SQL Server in macchine virtuali di Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Pagina del portale che fornisce collegamenti alla documentazione relativa all'esecuzione di SQL Server in Azure.
- [Scott Guthrie illustra i database SQL in Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). video introduttivo di 6 minuti al database SQL di Scott Guthrie.
- [Modelli di applicazione e strategie di sviluppo per SQL Server in macchine virtuali di Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Uso di Entity Framework e database SQL in un'app Web ASP.NET

- [Introduzione con EF 6 con MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Serie di esercitazioni in nove parti che illustra la creazione di un'app MVC che usa EF e distribuisce il database in Azure e nel database SQL.
- [Distribuzione Web ASP.NET con Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Serie di esercitazioni in dodici parti che approfondisce la distribuzione di un database usando EF Code First.
- [Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e database SQL in un sito Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Esercitazione dettagliata che illustra la creazione di un'app Web che usa l'autenticazione, archivia le tabelle dell'applicazione nel database delle appartenenze, modifica lo schema del database e distribuisce l'app in Azure.
- [Mappa del contenuto di accesso ai dati ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Collegamenti alle risorse per l'uso di EF e del database SQL.

Uso di MongoDB in Azure:

- [MongoLab-MongoDB in Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Pagina del portale per la documentazione sull'esecuzione di MongoDB in Azure.
- [Creare un sito Web di Azure che si connette a MongoDB in esecuzione in una macchina virtuale in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Esercitazione dettagliata che illustra come usare un database MongoDB in un'applicazione Web ASP.NET.

HDInsight (Hadoop in Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Documentazione del portale per HDInsight nel sito Web di [Azure](https://azure.microsoft.com/) .
- [Hadoop e HDInsight: Big data in Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Articolo di MSDN Magazine di Bruno Terkaly e Ricardo Villalobos, introduzione a Hadoop in Azure.
- [Modelli e procedure Microsoft: informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere il modello MapReduce.

> [!div class="step-by-step"]
> [Precedente](single-sign-on.md)
> [Successivo](data-partitioning-strategies.md)
