---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Caching distribuito (compilazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 87a7516415895e761d1589fd459b93e5c15c0f85
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456998"
---
# <a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Caching distribuito (compilazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Il capitolo precedente esaminava la gestione degli errori temporanei e citava la memorizzazione nella cache come strategia di interruttore. In questo capitolo vengono fornite informazioni aggiuntive sulla memorizzazione nella cache, ad esempio quando utilizzarlo, modelli comuni per utilizzarlo e come implementarlo in Azure.

## <a name="what-is-distributed-caching"></a>Informazioni sulla memorizzazione nella cache distribuita

Una cache fornisce accesso a bassa latenza e velocità effettiva elevata ai dati delle applicazioni di uso comune, archiviando i dati in memoria. Per un'app Cloud il tipo di cache più utile è la cache distribuita, ovvero i dati non vengono archiviati nella memoria del singolo server Web, ma in altre risorse cloud e i dati memorizzati nella cache vengono resi disponibili per tutti i server Web di un'applicazione (o altre macchine virtuali cloud che AR e utilizzato dall'applicazione).

![diagramma che Mostra più server Web che accedono agli stessi server di cache](distributed-caching/_static/image1.png)

Quando l'applicazione viene ridimensionata mediante l'aggiunta o la rimozione di server o quando i server vengono sostituiti a causa di aggiornamenti o errori, i dati memorizzati nella cache rimangono accessibili a ogni server che esegue l'applicazione.

Evitando l'accesso ai dati ad alta latenza di un archivio dati persistente, la memorizzazione nella cache può migliorare significativamente la velocità di risposta dell'applicazione. Il recupero dei dati dalla cache, ad esempio, è molto più veloce rispetto al recupero da un database relazionale.

Un vantaggio collaterale della memorizzazione nella cache è la riduzione del traffico verso l'archivio dati persistente, che può comportare costi ridotti quando sono previsti costi di uscita per l'archivio dati persistente.

## <a name="when-to-use-distributed-caching"></a>Quando usare la memorizzazione nella cache distribuita

La memorizzazione nella cache funziona meglio per i carichi di lavoro delle applicazioni che eseguono una maggiore lettura rispetto alla scrittura dei dati e quando il modello di dati supporta l'organizzazione chiave/valore usata per archiviare e recuperare i dati nella cache. È inoltre più utile quando gli utenti dell'applicazione condividono una grande quantità di dati comuni; ad esempio, la cache non fornirebbe molti vantaggi se ogni utente recupera in genere i dati univoci per tale utente. Un esempio in cui la memorizzazione nella cache potrebbe essere molto utile è un catalogo di prodotti, perché i dati non cambiano di frequente e tutti i clienti stanno esaminando gli stessi dati.

Il vantaggio della memorizzazione nella cache diventa sempre più misurabile, più la scalabilità di un'applicazione, poiché i limiti di velocità effettiva e i ritardi di latenza dell'archivio dati persistente diventano più un limite per le prestazioni complessive dell'applicazione. Tuttavia, è possibile implementare la memorizzazione nella cache per altri motivi rispetto alle prestazioni. Per i dati che non devono essere perfettamente aggiornati quando vengono visualizzati dall'utente, l'accesso alla cache può fungere da interruttore quando l'archivio dati persistente non risponde o non è disponibile.

## <a name="popular-cache-population-strategies"></a>Strategie comuni di popolamento della cache

Per poter recuperare i dati dalla cache, è necessario archiviarli per primi. Sono disponibili diverse strategie per ottenere i dati necessari in una cache:

- Su richiesta/cache a parte

    L'applicazione tenta di recuperare i dati dalla cache e, quando la cache non dispone dei dati (una "mancata"), l'applicazione archivia i dati nella cache in modo che siano disponibili alla prossima esecuzione. La volta successiva che l'applicazione tenta di ottenere gli stessi dati, trova la ricerca nella cache (un "hit"). Per impedire il recupero dei dati memorizzati nella cache che sono stati modificati nel database, è necessario invalidare la cache quando si apportano modifiche all'archivio dati.
- Push di dati in background

    I servizi in background effettuano il push dei dati nella cache in base a una pianificazione regolare e l'app estrae sempre dalla cache. Questo approccio funziona perfettamente con origini dati a latenza elevata che non richiedono che vengano restituiti sempre i dati più recenti.
- Interruttore

    L'applicazione comunica normalmente direttamente con l'archivio dati persistente, ma quando l'archivio dati persistente presenta problemi di disponibilità, l'applicazione recupera i dati dalla cache. È possibile che i dati siano stati inseriti nella cache usando la strategia di push dei dati in background o in background. Si tratta di una strategia di gestione degli errori piuttosto che di una strategia di miglioramento delle prestazioni.

Per rendere aggiornati i dati nella cache, è possibile eliminare le voci della cache correlate quando l'applicazione crea, aggiorna o Elimina i dati. Se è opportuno che l'applicazione a volte ottenga dati leggermente obsoleti, è possibile fare affidamento su una scadenza configurabile per impostare un limite per la quantità di dati della cache obsoleti.

È possibile configurare la scadenza assoluta (quantità di tempo dopo la creazione dell'elemento della cache) o la scadenza variabile (intervallo di tempo dall'ultima volta in cui è stato eseguito l'accesso a un elemento della cache). La scadenza assoluta viene utilizzata quando si dipende dal meccanismo di scadenza della cache per impedire che i dati diventino troppo obsoleti. Nell'app Correggi it verranno rimossi manualmente gli elementi della cache obsoleti e verrà usata la scadenza variabile per mantenerne i dati più aggiornati nella cache. Indipendentemente dai criteri di scadenza scelti, la cache eliminerà automaticamente gli elementi meno recenti (utilizzati meno di recente o LRU) quando viene raggiunto il limite di memoria della cache.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Esempio di codice cache-aside per la correzione dell'app it

Nell'esempio di codice seguente viene verificata prima la cache durante il recupero di un'attività Correggi it. Se l'attività viene rilevata nella cache, viene restituita; Se non viene trovato, viene ottenuto dal database e archiviato nella cache. Le modifiche apportate per aggiungere la memorizzazione nella cache al metodo `FindTaskByIdAsync` sono evidenziate.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Quando si aggiorna o si elimina un'attività Correggi it, è necessario invalidare (rimuovere) l'attività memorizzata nella cache. In caso contrario, i tentativi futuri di lettura dell'attività continueranno a recuperare i dati obsoleti dalla cache.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Questi sono esempi per illustrare il semplice codice di memorizzazione nella cache. la memorizzazione nella cache non è stata implementata nel progetto di correzione it scaricabile.

## <a name="azure-caching-services"></a>Servizi di Caching di Azure

Azure offre i servizi di memorizzazione nella cache seguenti: [cache Redis di Azure](https://msdn.microsoft.com/library/dn690523.aspx) e [cache gestita di Azure](https://msdn.microsoft.com/library/dn386094.aspx). Cache Redis di Azure si basa sulla popolare [cache Redis Open Source](http://redis.io/) ed è la prima scelta per la maggior parte degli scenari di Caching.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET lo stato della sessione utilizzando un provider di cache

Come indicato nel [capitolo procedure consigliate per lo sviluppo Web](web-development-best-practices.md), una procedura consigliata consiste nell'evitare di utilizzare lo stato della sessione. Se l'applicazione richiede lo stato della sessione, la procedura consigliata consiste nell'evitare il provider in memoria predefinito perché non Abilita scale out (più istanze del server Web). Il provider di stato della sessione SQL Server ASP.NET consente a un sito in esecuzione su più server Web di utilizzare lo stato della sessione, ma comporta un costo a latenza elevata rispetto a un provider in memoria. La soluzione migliore se è necessario usare lo stato della sessione consiste nell'usare un provider di cache, ad esempio il [provider di stato della sessione per cache di Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Riepilogo

È stato illustrato come l'app Fix it potrebbe implementare la memorizzazione nella cache per migliorare i tempi di risposta e la scalabilità e consentire all'app di continuare a rispondere per le operazioni di lettura quando il database non è disponibile. Nel [prossimo capitolo](queue-centric-work-pattern.md) verrà illustrato come migliorare ulteriormente la scalabilità e garantire che l'app continui a rispondere per le operazioni di scrittura.

## <a name="resources"></a>Risorse

Per ulteriori informazioni sulla memorizzazione nella cache, vedere le risorse seguenti.

Documentazione

- [Cache di Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentazione ufficiale di MSDN sulla memorizzazione nella cache in Azure.
- [Modelli e procedure Microsoft: informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere informazioni aggiuntive sulla memorizzazione nella cache e modello cache-aside.
- [Failsafe: linee guida per architetture cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper di Marc Mercuri, Ulrich Homann e Andrew Townhill. Vedere la sezione relativa alla memorizzazione nella cache.
- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi cloud di Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. White paper di Mark Simms e Michael Thomassy. Vedere la sezione relativa alla memorizzazione nella cache distribuita.
- [Memorizzazione nella cache distribuita nel percorso alla scalabilità](https://msdn.microsoft.com/magazine/dd942840.aspx). Un articolo di MSDN Magazine precedente (2009), ma una chiara Introduzione alla memorizzazione nella cache distribuita in generale. approfondisce le sezioni relative alla memorizzazione nella cache dei white paper di Safety e Best Practices.

Videos

- [Failsafe: compilazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Viene presentata una visualizzazione a livello di 400 di come progettare app cloud. Questa serie è incentrata sulla teoria e sui motivi per cui; per altre procedure dettagliate, vedere la pagina relativa alla creazione di grandi serie di Mark Simms. Vedere la discussione sulla memorizzazione nella cache in Episode 3 a partire da 1:24:14.
- [Creazione di grandi dimensioni: lezioni apprese dai clienti di Azure-parte i](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies illustra la memorizzazione nella cache distribuita a partire da 46:00. In modo analogo alla serie failsafe, ma passa ad altre procedure dettagliate. La presentazione è stata data il 31 ottobre 2012, quindi non copre il servizio di memorizzazione nella cache di app Web nel servizio app Azure introdotto in 2013.

Esempio di codice

- [Nozioni fondamentali sui servizi cloud in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Applicazione di esempio che implementa la memorizzazione nella cache distribuita. Vedere il post di Blog relativo alle [nozioni di base del servizio cloud-nozioni fondamentali sulla memorizzazione nella cache](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Precedente](transient-fault-handling.md)
> [Successivo](queue-centric-work-pattern.md)
