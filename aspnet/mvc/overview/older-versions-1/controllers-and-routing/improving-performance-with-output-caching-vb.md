---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: Miglioramento delle prestazioni con la cache di output (VB) | Microsoft Docs
author: microsoft
description: In questa esercitazione si apprenderà come migliorare significativamente le prestazioni delle applicazioni Web ASP.NET MVC sfruttando la memorizzazione nella cache di output. ...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: b713b56e149f196794b3223ba88e3b41bf3e34c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601248"
---
# <a name="improving-performance-with-output-caching-vb"></a>Miglioramento delle prestazioni con la cache di output (VB)

[Microsoft](https://github.com/microsoft)

> In questa esercitazione si apprenderà come migliorare significativamente le prestazioni delle applicazioni Web ASP.NET MVC sfruttando la memorizzazione nella cache di output. Si apprenderà come memorizzare nella cache il risultato restituito da un'azione del controller, in modo che non sia necessario creare lo stesso contenuto ogni volta che un nuovo utente richiama l'azione.

L'obiettivo di questa esercitazione è spiegare come è possibile migliorare notevolmente le prestazioni di un'applicazione MVC ASP.NET sfruttando la cache di output. La cache di output consente di memorizzare nella cache il contenuto restituito da un'azione del controller. In questo modo, non è necessario che lo stesso contenuto venga generato ogni volta che viene richiamata la stessa azione del controller.

Si supponga, ad esempio, che l'applicazione ASP.NET MVC visualizzi un elenco di record di database in una vista denominata index. In genere, ogni volta che un utente richiama l'azione del controller che restituisce la vista index, è necessario recuperare il set di record del database dal database eseguendo una query sul database.

Se, d'altra parte, si sfrutta la cache di output, è possibile evitare di eseguire una query di database ogni volta che un utente richiama la stessa azione del controller. La vista può essere recuperata dalla cache anziché essere rigenerata dall'azione del controller. La memorizzazione nella cache consente di evitare l'esecuzione di operazioni ridondanti sul server.

#### <a name="enabling-output-caching"></a>Abilitazione della memorizzazione nella cache di output

Per abilitare la memorizzazione nella cache dell'output, è necessario aggiungere un &lt;OutputCache&gt; attributo a una singola azione del controller o a un'intera classe controller. Il controller nel listato 1, ad esempio, espone un'azione denominata index (). L'output dell'azione index () viene memorizzato nella cache per 10 secondi.

**Listato 1 – Controllers\HomeController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]

Nelle versioni beta di ASP.NET MVC, la memorizzazione nella cache di output non funziona per un URL come [http://www.MySite.com/](http://www.mysite.com/). È invece necessario immettere un URL come [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index).

Nel listato 1, l'output dell'azione index () viene memorizzato nella cache per 10 secondi. Se si preferisce, è possibile specificare una durata della cache molto più lunga. Ad esempio, se si desidera memorizzare nella cache l'output di un'azione del controller per un giorno, è possibile specificare una durata della cache di 86400 secondi (60 secondi \* 60 minuti \* 24 ore).

Non vi è alcuna garanzia che il contenuto venga memorizzato nella cache per il periodo di tempo specificato. Quando le risorse di memoria diventano insufficienti, la cache inizia a rimuovere automaticamente il contenuto.

Il controller Home nel listato 1 restituisce la vista index nel listato 2. Non c'è niente di speciale in questa visualizzazione. La visualizzazione indice Visualizza semplicemente l'ora corrente (vedere la figura 1).

**Listato 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

**Figura 1: visualizzazione dell'indice memorizzato nella cache**

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

Se si richiama l'azione index () più volte immettendo l'URL/Home/Index nella barra degli indirizzi del browser e premendo ripetutamente il pulsante Aggiorna/ricarica nel browser, l'ora visualizzata dalla vista index non cambierà per 10 secondi. La stessa ora viene visualizzata perché la vista è memorizzata nella cache.

È importante comprendere che la stessa visualizzazione è memorizzata nella cache per tutti gli utenti che visitano l'applicazione. Tutti gli utenti che richiamano l'azione index () otterranno la stessa versione memorizzata nella cache della vista index. Ciò significa che la quantità di lavoro che il server Web deve eseguire per gestire la visualizzazione dell'indice è notevolmente ridotta.

La visualizzazione nel listato 2 si sta svolgendo un'operazione molto semplice. La visualizzazione Visualizza solo l'ora corrente. Tuttavia, è possibile memorizzare nella cache una vista che visualizza un set di record di database. In tal caso, non è necessario recuperare il set di record del database dal database ogni volta che viene richiamata l'azione del controller che restituisce la visualizzazione. La memorizzazione nella cache può ridurre la quantità di lavoro che il server Web e il server di database devono eseguire.

Non utilizzare la pagina &lt;direttiva% @ OutputCache%&gt; in una visualizzazione MVC. Questa direttiva sta per essere sottoporre a Bleeding dal mondo Web Form e non deve essere usata in un'applicazione MVC ASP.NET. 

#### <a name="where-content-is-cached"></a>Posizione in cui il contenuto viene memorizzato nella cache

Per impostazione predefinita, quando si usa l'attributo &lt;OutputCache&gt;, il contenuto viene memorizzato nella cache in tre posizioni: il server Web, i server proxy e il Web browser. È possibile controllare esattamente la posizione in cui il contenuto viene memorizzato nella cache modificando la proprietà Location dell'attributo &lt;OutputCache&gt;.

È possibile impostare la proprietà Location su uno dei valori seguenti:

> · Qualsiasi
> 
> · Client
> 
> · Downstream
> 
> · Server
> 
> · Nessuno
> 
> · ServerAndClient

Per impostazione predefinita, la proprietà location ha il valore any. In alcune situazioni, tuttavia, potrebbe essere necessario memorizzare nella cache solo sul browser o solo sul server. Ad esempio, se si memorizzano nella cache informazioni personalizzate per ogni utente, è consigliabile non memorizzare nella cache le informazioni sul server. Se si visualizzano informazioni diverse per utenti diversi, è necessario memorizzare nella cache le informazioni solo sul client.

Il controller nel listato 3, ad esempio, espone un'azione denominata GetName () che restituisce il nome utente corrente. Se Jack accede al sito Web e richiama l'azione GetName (), l'azione restituisce la stringa "Hi Jack". Se, successivamente, Jill accede al sito Web e richiama l'azione GetName (), otterrà anche la stringa "Hi Jack". La stringa viene memorizzata nella cache del server Web per tutti gli utenti dopo che Jack richiama inizialmente l'azione del controller.

**Listato 3 – Controllers\BadUserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

Probabilmente, il controller nel listato 3 non funziona nel modo desiderato. Non si vuole visualizzare il messaggio "Hi Jack" per Jill.

Non inserire mai nella cache il contenuto personalizzato nella cache del server. Tuttavia, potrebbe essere necessario memorizzare nella cache il contenuto personalizzato nella cache del browser per migliorare le prestazioni. Se si memorizza nella cache il contenuto del browser e un utente richiama la stessa azione del controller più volte, il contenuto può essere recuperato dalla cache del browser invece che dal server.

Il controller modificato nel listato 4 memorizza nella cache l'output dell'azione GetName (). Tuttavia, il contenuto viene memorizzato nella cache solo nel browser e non nel server. In questo modo, quando più utenti richiamano il metodo GetName (), ogni persona ottiene il proprio nome utente e non il nome utente di un'altra persona.

**Listato 4-Controllers\UserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

Si noti che l'&lt;OutputCache&gt; attributo nel listato 4 include una proprietà location impostata sul valore OutputCacheLocation. client. L'attributo &lt;OutputCache&gt; include anche una proprietà NoStore. La proprietà NoStore viene utilizzata per informare i server proxy e i browser che non devono archiviare una copia permanente del contenuto memorizzato nella cache.

#### <a name="varying-the-output-cache"></a>Variazione della cache di output

In alcuni casi, è possibile che si vogliano diverse versioni memorizzate nella cache dello stesso contenuto. Si supponga, ad esempio, di creare una pagina master/dettagli. Nella pagina master viene visualizzato un elenco di titoli di film. Quando si fa clic su un titolo, si ottengono i dettagli per il film selezionato.

Se si memorizza nella cache la pagina dei dettagli, verranno visualizzati i dettagli per lo stesso film indipendentemente dal film selezionato. Il primo film selezionato dal primo utente verrà visualizzato a tutti gli utenti futuri.

È possibile risolvere questo problema sfruttando la proprietà VaryByParam dell'attributo &lt;OutputCache&gt;. Questa proprietà consente di creare diverse versioni memorizzate nella cache dello stesso contenuto quando un parametro del form o un parametro della stringa di query varia.

Ad esempio, il controller nel listato 5 espone due azioni denominate Master () e Details (). L'azione Master () restituisce un elenco di titoli di film e l'azione dettagli () restituisce i dettagli per il film selezionato.

**Listato 5 – Controllers\MoviesController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

L'azione Master () include una proprietà VaryByParam con il valore "None". Quando viene richiamata l'azione Master (), viene restituita la stessa versione memorizzata nella cache della visualizzazione master. Tutti i parametri del modulo o i parametri della stringa di query vengono ignorati (vedere la figura 2).

**Figura 2: visualizzazione/Movies/Master**

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

**Figura 3: visualizzazione/Movies/Details**

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

L'azione Details () include una proprietà VaryByParam con il valore "ID". Quando vengono passati valori diversi del parametro ID all'azione del controller, vengono generate diverse versioni memorizzate nella cache della visualizzazione dettagli.

È importante comprendere che l'utilizzo della proprietà VaryByParam comporta un maggior numero di Caching e non meno. Viene creata una diversa versione memorizzata nella cache della visualizzazione dettagli per ogni versione diversa del parametro ID.

È possibile impostare la proprietà VaryByParam sui valori seguenti:

> \* = crea una versione diversa memorizzata nella cache ogni volta che un modulo o un parametro della stringa di query varia.
> 
> nessuno = non crea mai diverse versioni memorizzate nella cache
> 
> Elenco punti e virgola di parametri = crea diverse versioni memorizzate nella cache ogni volta che i parametri del form o della stringa di query nell'elenco variano

#### <a name="creating-a-cache-profile"></a>Creazione di un profilo della cache

In alternativa alla configurazione delle proprietà della cache di output modificando le proprietà dell'attributo &lt;OutputCache&gt;, è possibile creare un profilo della cache nel file di configurazione Web (Web. config). La creazione di un profilo della cache nel file di configurazione Web offre alcuni importanti vantaggi.

Innanzitutto, configurando la memorizzazione nella cache di output nel file di configurazione Web, è possibile controllare il modo in cui le azioni del controller memorizzano nella cache il contenuto in una posizione È possibile creare un profilo della cache e applicare il profilo a diversi controller o azioni del controller.

In secondo luogo, è possibile modificare il file di configurazione Web senza ricompilare l'applicazione. Se è necessario disabilitare la memorizzazione nella cache per un'applicazione che è già stata distribuita nell'ambiente di produzione, è possibile modificare semplicemente i profili della cache definiti nel file di configurazione Web. Eventuali modifiche apportate al file di configurazione Web verranno rilevate automaticamente e applicate.

Ad esempio, la sezione relativa alla configurazione di &lt;Caching&gt; Web nel listato 6 definisce un profilo della cache denominato Cache1Hour. La sezione &lt;Caching&gt; deve essere visualizzata nella sezione &lt;&gt; System. Web di un file di configurazione Web.

**Elenco 6-sezione caching per Web. config**

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

Il controller nel listato 7 illustra come applicare il profilo Cache1Hour a un'azione del controller con il &lt;OutputCache&gt; attributo.

**Listato 7 – Controllers\ProfileController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

Se si richiama l'azione index () esposta dal controller nel listato 7, la stessa ora verrà restituita per 1 ora.

#### <a name="summary"></a>Riepilogo

La memorizzazione nella cache di output fornisce un metodo molto semplice per migliorare in modo significativo le prestazioni delle applicazioni MVC ASP.NET. In questa esercitazione si è appreso come usare l'attributo&gt; OutputCache di &lt;per memorizzare nella cache l'output delle azioni del controller. Si è inoltre appreso come modificare le proprietà del &lt;OutputCache&gt; attributo, ad esempio la durata e le proprietà VaryByParam, per modificare la modalità di memorizzazione nella cache del contenuto. Infine, si è appreso come definire i profili della cache nel file di configurazione Web.

> [!div class="step-by-step"]
> [Precedente](understanding-action-filters-vb.md)
> [Successivo](adding-dynamic-content-to-a-cached-page-vb.md)
