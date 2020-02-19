---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Procedure consigliate per lo sviluppo Web (compilazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: dfd8a3ac2328d3f17dfbe36e68b37d181177b0f4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457089"
---
# <a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Procedure consigliate per lo sviluppo Web (compilazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

I primi tre modelli erano la configurazione di un processo di sviluppo agile. il resto riguarda l'architettura e il codice. Questo è un insieme di procedure consigliate per lo sviluppo Web:

- [Server Web](#stateless) senza stato protetti da un servizio di bilanciamento del carico intelligente.
- [Evitare lo stato della sessione](#sessionstate) o, se non è possibile evitarlo, utilizzare la cache distribuita anziché un database.
- [Usare una rete CDN](#cdn) per asset di file statici con cache perimetrale (immagini, script).
- [Usare il supporto asincrono di .NET 4.5](#async) per evitare di bloccare le chiamate.

Queste procedure sono valide per tutti gli sviluppatori Web, non solo per le app Cloud, ma sono particolarmente importanti per le app cloud. Collaborano per consentire un uso ottimale della scalabilità altamente flessibile offerta dall'ambiente cloud. Se non si seguono queste procedure, si verificano limitazioni quando si tenta di ridimensionare l'applicazione.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Livello Web senza stato dietro un servizio di bilanciamento del carico intelligente

Il *livello Web* senza stato significa che non vengono archiviati dati dell'applicazione nella memoria del server web o file System. Mantenere il livello Web senza stato consente di offrire un'esperienza utente migliore e risparmiare denaro:

- Se il livello Web è senza stato e si trova dietro un servizio di bilanciamento del carico, è possibile rispondere rapidamente alle modifiche nel traffico dell'applicazione, aggiungendo o rimuovendo in modo dinamico i server. Nell'ambiente cloud in cui si pagano solo le risorse del server per tutto il tempo in cui vengono effettivamente usati, la possibilità di rispondere alle modifiche nella domanda può tradursi in un notevole risparmio.
- Un livello Web senza stato è molto più semplice per la scalabilità orizzontale dell'applicazione. Questo consente anche di rispondere più rapidamente alle esigenze di scalabilità e di spendere meno denaro per lo sviluppo e il test nel processo.
- I server cloud, come i server locali, devono essere corretti e riavviati occasionalmente. Se il livello Web è senza stato, il routing del traffico quando un server diventa temporaneamente inattivo non provoca errori o comportamenti imprevisti.

Per la maggior parte delle applicazioni reali è necessario archiviare lo stato per una sessione Web. il punto principale è quello di non archiviarlo sul server Web. È possibile archiviare lo stato in altri modi, ad esempio nel client in cookie o sul lato server out-of-process nello stato della sessione ASP.NET usando un provider di cache. È possibile archiviare i file nell'archivio [BLOB di Windows Azure](unstructured-blob-storage.md) invece che nel file system locale.

Come esempio di come è possibile ridimensionare un'applicazione in siti Web di Windows Azure se il livello Web è senza stato, vedere la scheda **scala** per un sito Web di Microsoft Azure nel portale di gestione:

![Scheda Scalabilità](web-development-best-practices/_static/image1.png)

Se si desidera aggiungere server Web, è sufficiente trascinare il dispositivo di scorrimento conteggio istanze a destra. Impostarla su 5 e fare clic su **Save (Salva**) e, in pochi secondi, si dispone di 5 server Web in Windows Azure che gestiscono il traffico del sito Web.

![Cinque istanze](web-development-best-practices/_static/image2.png)

È possibile impostare il numero di istanze su 3 o su 1 in modo semplice. Quando si esegue la scalabilità, si inizia subito a risparmiare denaro perché Windows Azure viene addebitato al minuto, non all'ora.

È inoltre possibile indicare a Windows Azure di aumentare o diminuire automaticamente il numero di server Web in base all'utilizzo della CPU. Nell'esempio seguente, quando l'utilizzo della CPU scende al di sotto del 60%, il numero di server Web verrà ridotto a un minimo di 2 e se l'utilizzo della CPU supera il 80%, il numero di server Web verrà aumentato fino a un massimo di 4.

![Ridimensiona in base all'utilizzo della CPU](web-development-best-practices/_static/image3.png)

O se si è certi che il sito sarà occupato solo durante le ore lavorative? È possibile fare in modo che Windows Azure esegua più server durante il giorno e Riduci a un singolo server serate, notti e fine settimana. La serie seguente di schermate Mostra come configurare il sito Web per l'esecuzione di un server in ore non lavorative e 4 server durante le ore lavorative dalle 8.00 alle 17.00.

![Ridimensiona in base alla pianificazione](web-development-best-practices/_static/image4.png)

![Imposta ore di pianificazione](web-development-best-practices/_static/image5.png)

![Pianificazione diurna](web-development-best-practices/_static/image6.png)

![Pianificazione di weeknight](web-development-best-practices/_static/image7.png)

![Pianificazione fine settimana](web-development-best-practices/_static/image8.png)

Naturalmente tutto questo può essere eseguito anche negli script e nel portale.

La capacità di scalabilità orizzontale dell'applicazione è quasi illimitata in Windows Azure, purché si evitino gli ostacoli per aggiungere o rimuovere in modo dinamico le macchine virtuali del server, mantenendo lo stato del livello Web.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evitare lo stato della sessione

In un'app reale per il cloud spesso non è facile evitare di archiviare qualche forma di stato per una sessione utente, ma alcuni approcci hanno più effetto di altri sulle prestazioni e sulla scalabilità. Se è necessario archiviare lo stato, la soluzione migliore è mantenere piccola la quantità di stato e archiviarla nei cookie. Se ciò non è fattibile, la soluzione migliore successiva consiste nell'usare lo stato della sessione ASP.NET con un provider per la [cache distribuita in memoria](distributed-caching.md#sessionstate). La soluzione peggiore dal punto di vista delle prestazioni e della scalabilità è usare un provider di stato della sessione supportato da un database.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Usare una rete CDN per memorizzare nella cache asset di file statici

La rete CDN è un acronimo della rete per la distribuzione di contenuti. Si forniscono asset di file statici, ad esempio immagini e file di script, a un provider della rete CDN e il provider memorizza nella cache i file nei data center in tutto il mondo, in modo che ogni volta che gli utenti accedano all'applicazione, ottengano una risposta relativamente rapida e bassa latenza per la cache Asset. In questo modo si accelera il tempo di caricamento complessivo del sito e il carico sui server Web viene ridotto. CDNs sono particolarmente importanti se si sta raggiungendo un pubblico ampiamente distribuito geograficamente.

Windows Azure dispone di una rete CDN ed è possibile utilizzare altri CDNs in un'applicazione eseguita in Windows Azure o in un qualsiasi ambiente di hosting Web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Usare il supporto asincrono di .NET 4.5 per evitare il blocco delle chiamate

.NET 4,5 ha migliorato C# i linguaggi di programmazione e VB per semplificare la gestione delle attività in modo asincrono. Il vantaggio della programmazione asincrona non riguarda solo le situazioni di elaborazione parallela, ad esempio quando si vuole avviare più chiamate al servizio Web simultaneamente. Consente inoltre al server Web di eseguire in modo più efficiente e affidabile in condizioni di carico elevato. Un server Web dispone solo di un numero limitato di thread disponibili e, in condizioni di carico elevato, quando tutti i thread sono in uso, le richieste in ingresso devono attendere fino a quando i thread non vengono liberati. Se il codice dell'applicazione non gestisce attività come le query del database e le chiamate del servizio Web in modo asincrono, molti thread sono inutilmente vincolati mentre il server è in attesa di una risposta di I/O. Questo limita la quantità di traffico che il server è in grado di gestire in condizioni di carico elevato. Con la programmazione asincrona, i thread in attesa che un servizio Web o un database restituiscano dati vengono liberati fino a soddisfare nuove richieste fino a quando non vengono ricevuti i dati. In un server Web occupato, centinaia o migliaia di richieste possono essere elaborate tempestivamente, che altrimenti sarebbero in attesa della liberazione dei thread.

Come si è visto in precedenza, è facile ridurre il numero di server Web che gestiscono il sito Web in modo da aumentarli. Quindi, se un server è in grado di ottenere una maggiore velocità effettiva, non è necessario il maggior numero di questi ed è possibile ridurre i costi in quanto è necessario un minor numero di server per un determinato volume di traffico rispetto a quanto altrimenti.

Il supporto per il modello di programmazione asincrona di .NET 4,5 è incluso in ASP.NET 4,5 per Web Form, MVC e l'API Web. in Entity Framework 6 e nell'API di [archiviazione di Microsoft Azure](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Supporto asincrono in ASP.NET 4,5

In ASP.NET 4,5 è stato aggiunto il supporto per la programmazione asincrona non solo per il linguaggio, ma anche per i framework MVC, Web Form e API Web. Ad esempio, un metodo di azione del controller MVC ASP.NET riceve i dati da una richiesta Web e passa i dati a una visualizzazione che quindi crea il codice HTML da inviare al browser. Spesso il metodo di azione deve recuperare i dati da un database o da un servizio Web per visualizzarli in una pagina Web o per salvare i dati immessi in una pagina Web. In questi scenari è facile rendere asincrono il metodo di azione: anziché restituire un oggetto *ActionResult* , si restituisce *Task&lt;ActionResult&gt;* e si contrassegna il metodo con la parola chiave *Async* . All'interno del metodo, quando una riga di codice avvia un'operazione che implica il tempo di attesa, contrassegnarla con la parola chiave await.

Di seguito è riportato un semplice metodo di azione che chiama un metodo del repository per una query di database:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

E questo è lo stesso metodo che gestisce la chiamata al database in modo asincrono:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

In viene illustrata la generazione del codice asincrono appropriato da parte del compilatore. Quando l'applicazione effettua la chiamata a `FindTaskByIdAsync`, ASP.NET esegue la richiesta di `FindTask` e quindi rimuove il thread di lavoro e lo rende disponibile per l'elaborazione di un'altra richiesta. Quando viene eseguita la richiesta di `FindTask`, viene riavviato un thread per continuare l'elaborazione del codice che viene eseguito dopo la chiamata. Durante l'intervallo tra il momento in cui viene avviata la richiesta di `FindTask` e il momento in cui vengono restituiti i dati, è disponibile un thread per eseguire operazioni utili che altrimenti verrebbero vincolate in attesa della risposta.

Si verifica un sovraccarico per il codice asincrono, ma in condizioni di basso carico l'overhead è trascurabile, mentre in condizioni di carico elevato è possibile elaborare le richieste che altrimenti verrebbero mantenute in attesa dei thread disponibili.

È possibile eseguire questo tipo di programmazione asincrona a partire da ASP.NET 1,1, ma è difficile scrivere, soggette a errori e difficili da eseguire il debug. Ora che il codice è stato semplificato in ASP.NET 4,5, non c'è niente di più.

### <a name="async-support-in-entity-framework-6"></a>Supporto asincrono in Entity Framework 6

Come parte del supporto asincrono in 4,5 è stato fornito il supporto asincrono per le chiamate ai servizi Web, I socket e l'I/O file system, ma il modello più comune per le applicazioni Web è quello di raggiungere un database e le librerie di dati non supportano Async. A questo punto Entity Framework 6 aggiunge il supporto asincrono per l'accesso al database.

In Entity Framework 6 tutti i metodi che determinano l'invio di una query o di un comando al database hanno versioni asincrone. Nell'esempio riportato di seguito viene illustrata la versione asincrona del metodo *Find* .

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Questo supporto asincrono funziona non solo per gli inserimenti, le eliminazioni, gli aggiornamenti e le ricerche semplici. funziona anche con le query LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Esiste una versione `Async` del metodo `ToList` perché in questo codice è il metodo che causa l'invio di una query al database. I metodi `Where` e `OrderByDescending` configurano solo la query, mentre il metodo `ToListAsync` esegue la query e archivia la risposta nella variabile `result`.

## <a name="summary"></a>Riepilogo

È possibile implementare le procedure consigliate per lo sviluppo Web descritte qui in qualsiasi framework di programmazione Web e in qualsiasi ambiente cloud, ma sono disponibili strumenti in ASP.NET e Windows Azure per semplificare l'operazione. Se si seguono questi modelli, è possibile scalare facilmente il livello Web e ridurre al minimo le spese perché ogni server sarà in grado di gestire più traffico.

Il [capitolo successivo](single-sign-on.md) analizza il modo in cui il cloud abilita Single Sign-on scenari.

## <a name="resources"></a>Risorse

Per ulteriori informazioni, vedere le risorse seguenti.

Server Web senza stato:

- [Modelli e procedure Microsoft: linee guida per la scalabilità](https://msdn.microsoft.com/library/dn589774.aspx)automatica.
- [Disabilitazione dell'affinità di istanze di Arr nei siti Web di Microsoft Azure](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Il post di Blog di Erez BenAri, illustra l'affinità di sessione in siti Web di Microsoft Azure.

CDN:

- [Failsafe: compilazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie di video in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Vedere la discussione sulla rete CDN nell'episodio 3 a partire da 1:34:00.
- [Modello di hosting del contenuto statico di Microsoft Patterns and Practices](https://msdn.microsoft.com/library/dn589776.aspx)
- [Revisioni](http://www.cdnreviews.com/)della rete CDN. Panoramica di molti CDNs.

Programmazione asincrona:

- [Uso di metodi asincroni in ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Esercitazione di Rick Anderson.
- [Programmazione asincrona con Async e await (C# e Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). MSDN white paper che spiega la logica per la programmazione asincrona, il funzionamento in ASP.NET 4,5 e come scrivere codice per implementarlo.
- [Entity Framework query asincrona e Salva](https://msdn.microsoft.com/data/jj819165)
- [Come compilare applicazioni Web ASP.NET con Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Presentazione video di Rowan Miller. Include una dimostrazione grafica del modo in cui la programmazione asincrona può facilitare gli aumenti significativi della velocità effettiva del server Web in condizioni di carico elevato.
- [Failsafe: compilazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie di video in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Per le discussioni sull'effetto della programmazione asincrona sulla scalabilità, vedere Episode 4 e Episode 8.
- [La magia di usare i metodi asincroni in ASP.NET 4,5, più un punto di errore importante](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Post di Blog di Scott Hanselt, principalmente sull'uso di async nelle applicazioni Web Form ASP.NET.

Per altre procedure consigliate per lo sviluppo Web, vedere le risorse seguenti:

- [Procedure consigliate per la correzione dell'applicazione di esempio](the-fix-it-sample-application.md#bestpractices). L'appendice di questo e-book elenca una serie di procedure consigliate implementate nell'applicazione Correggi it.
- [Elenco di controllo per sviluppatori Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Precedente](continuous-integration-and-continuous-delivery.md)
> [Successivo](single-sign-on.md)
