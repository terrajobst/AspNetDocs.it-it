---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Panoramica del progetto Katana | Microsoft Docs
author: howarddierking
description: Il framework ASP.NET è stato girato da più di dieci anni e la piattaforma ha permesso lo sviluppo di innumerevoli siti e servizi Web. Come applicazione Intelligence Web...
ms.author: riande
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 1f28db822930cdfd2ebf4cf9bb27d173f4aa4201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617236"
---
# <a name="an-overview-of-project-katana"></a>Panoramica del progetto Katana

di [Howard Dierking](https://github.com/howarddierking)

> Il framework ASP.NET è stato girato da più di dieci anni e la piattaforma ha permesso lo sviluppo di innumerevoli siti e servizi Web. Man mano che le strategie di sviluppo delle applicazioni Web si sono evolute, il Framework è stato in grado di evolversi con tecnologie come ASP.NET MVC e API Web ASP.NET. Dato che lo sviluppo di applicazioni Web prende il passo avanti nel mondo di cloud computing, Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) fornisce il set di componenti sottostante per ASP.NET le applicazioni, consentendo loro di essere flessibili, portabili, leggeri e offrire prestazioni migliori, in modo da poterli creare in un altro modo, Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) cloud ottimizza le applicazioni ASP.NET.

## <a name="why-katana--why-now"></a>Perché Katana: perché adesso?

 Indipendentemente dal fatto che si tratti di un Framework per sviluppatori o di un prodotto per l'utente finale, è importante comprendere le motivazioni sottostanti per la creazione del prodotto, che include la conoscenza della creazione del prodotto. ASP.NET è stato originariamente creato tenendo conto di due clienti.   
  
**Il primo gruppo di clienti era costituito da sviluppatori ASP classici.** Al momento, ASP era una delle tecnologie principali per la creazione di applicazioni e siti Web dinamici basati sui dati mediante l'intreccio di markup e script sul lato server. Il runtime ASP ha fornito uno script sul lato server con un set di oggetti che astraggono gli aspetti principali del protocollo HTTP sottostante e del server Web e fornisce l'accesso a servizi aggiuntivi, ad esempio la gestione dello stato dell'applicazione, la sessione e la cache e così via. Anche se potenti, le applicazioni ASP classiche sono diventate una sfida da gestire Man mano che aumentano le dimensioni e la complessità. Questo è stato in gran parte dovuto alla mancanza di struttura presente negli ambienti di scripting abbinato alla duplicazione del codice risultante dall'interfoliazione del codice e del markup. Per sfruttare al meglio i punti di forza di ASP classico, affrontando alcune delle proprie esigenze, ASP.NET ha sfruttato l'organizzazione del codice fornita dai linguaggi orientati agli oggetti dei .NET Framework mantenendo al tempo stesso il modello di programmazione lato server per cui gli sviluppatori ASP classici erano abituati.

**Il secondo gruppo di clienti di destinazione per ASP.NET era Windows Business Application Developers.** A differenza degli sviluppatori ASP classici, che erano abituati a scrivere markup HTML e il codice per generare un maggior numero di markup HTML, gli sviluppatori di WinForms (come gli sviluppatori VB6 prima di essi) erano abituati a un'esperienza di progettazione che includeva un'area di disegno e un set completo di utenti controlli dell'interfaccia. La prima versione di ASP.NET, nota anche come "Web Form", offre un'esperienza di progettazione simile, oltre a un modello di eventi sul lato server per i componenti dell'interfaccia utente e un set di funzionalità dell'infrastruttura, come ViewState, per creare un'esperienza di sviluppo senza problemi. tra la programmazione lato client e server. I Web Form hanno effettivamente nascosto la natura senza stato del Web in un modello di eventi con stato che era familiare agli sviluppatori di Windows Form.

### <a name="challenges-raised-by-the-historical-model"></a>Problemi generati dal modello cronologico

**Il risultato finale era un modello di programmazione per sviluppatori e Runtime completo e ricco di funzionalità.** Con questa funzionalità, tuttavia, sono emerse due importanti problemi. In primo luogo, il Framework era **monolitico**, con unità di funzionalità logicamente separate e strettamente collegate nello stesso assembly System. Web. dll (ad esempio, gli oggetti http di base con il framework Web Form). In secondo luogo, ASP.NET è stato incluso come parte del .NET Framework più grande, il che significava che l' **intervallo tra le versioni era nell'ordine degli anni.** Questo ha reso difficile per ASP.NET rimanere al passo con tutte le modifiche apportate nello sviluppo Web in rapida evoluzione. Infine, System. Web. dll è stato associato in diversi modi a una specifica opzione di hosting Web: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Passaggi evoluzionari: ASP.NET MVC e API Web ASP.NET

Sono state apportate numerose modifiche nello sviluppo Web. Le applicazioni Web sono state sempre sviluppate come una serie di piccoli componenti mirati piuttosto che di grandi Framework. Il numero di componenti e la frequenza con cui sono stati rilasciati aumentavano a una velocità sempre maggiore. È stato chiaro che la velocità con il Web richiederebbe Framework per ottenere i Framework più piccoli, separati e più mirati, anziché più grandi e più ricchi di funzionalità, quindi il **team di ASP.NET ha adottato diversi passaggi evoluzionari per abilitare ASP.NET come una famiglia di componenti Web innestabili piuttosto che come un unico framework**.

Una delle prime modifiche è stata la popolarità del modello di progettazione MVC (Model-View-Controller) noto grazie ai Framework di sviluppo Web, come Ruby on Rails. Questo stile di creazione di applicazioni Web ha dato allo sviluppatore un maggiore controllo sul markup dell'applicazione, mantenendo al tempo stesso la separazione del markup e della logica di business, che era uno dei punti di vendita iniziali per ASP.NET. Per soddisfare la domanda di questo stile di sviluppo di applicazioni Web, Microsoft ha avuto la possibilità di posizionarsi meglio per il futuro **sviluppando ASP.NET MVC fuori banda** (senza includerlo nella .NET Framework). ASP.NET MVC è stato rilasciato come download indipendente. In questo modo, il team di progettazione ha la flessibilità di distribuire gli aggiornamenti molto più spesso di quanto fosse possibile in precedenza.

Un altro passaggio importante nello sviluppo di applicazioni Web era il passaggio da pagine Web dinamiche generate dal server a markup iniziale statico con sezioni dinamiche della pagina generate dallo script sul lato client che comunicava **con le API Web back-end tramite richieste AJAX**. Questo cambiamento di architettura ha aiutato a spingere l'ascesa delle API Web e lo sviluppo di API Web ASP.NET Framework. Come nel caso di ASP.NET MVC, il rilascio di API Web ASP.NET ha fornito un'altra opportunità per evolvere ulteriormente ASP.NET come Framework più modulare. Il team di progettazione ha sfruttato la possibilità e ha **creato API Web ASP.NET in modo che non abbia dipendenze da nessuno dei tipi di Framework principali disponibili in System. Web. dll**. Questa operazione ha consentito due elementi: prima di tutto, significava che API Web ASP.NET possibile evolvere in modo completamente autonomo (e potrebbe continuare a eseguire l'iterazione rapidamente perché viene recapitato tramite NuGet). In secondo luogo, poiché non sono presenti dipendenze esterne da System. Web. dll e pertanto non esistono dipendenze da IIS, API Web ASP.NET inclusa la funzionalità per l'esecuzione in un host personalizzato, ad esempio un'applicazione console, un servizio Windows e così via.

### <a name="the-future-a-nimble-framework"></a>Il futuro: un framework Agile

Separando i componenti del Framework tra loro e quindi rilasciandoli in NuGet, i Framework potrebbero ora eseguire un' **iterazione in modo più indipendente e più rapido**. Inoltre, la potenza e la flessibilità della funzionalità self-hosting dell'API Web si sono rivelate molto attraenti per gli sviluppatori che volevano un **piccolo host leggero** per i servizi. Si è rivelato così interessante, in realtà, che anche altri Framework volevano questa funzionalità e questo è emerso un nuovo problema in quanto ogni Framework è stato eseguito nel proprio processo host nel proprio indirizzo di base e doveva essere gestito (avviato, interrotto e così via) in modo indipendente. Un'applicazione Web moderna, in genere, supporta il servizio file statico, la generazione dinamica di pagine, l'API Web e le notifiche push e in tempo reale più recenti. Si prevede che ognuno di questi servizi deve essere eseguito e gestito in modo indipendente e non è semplicemente realistico.

Ciò che era necessario era una singola astrazione di hosting che consentiva agli sviluppatori di comporre un'applicazione da un'ampia gamma di componenti e Framework diversi, quindi eseguire l'applicazione in un host di supporto.

## <a name="the-open-web-interface-for-net-owin"></a>Interfaccia Web aperta per .NET (OWIN)

 Ispirati ai vantaggi realizzati da [rack](http://rack.github.io/) nella community Ruby, diversi membri della community .NET hanno deciso di creare un'astrazione tra i server Web e i componenti del Framework. Due obiettivi di progettazione per l'astrazione OWIN sono stati semplici e hanno richiesto il minor numero possibile di dipendenze da altri tipi di Framework. Questi due obiettivi contribuiscono a garantire quanto segue:

- I nuovi componenti possono essere sviluppati e utilizzati in modo più semplice.
- È possibile trasferire più facilmente le applicazioni tra gli host e le piattaforme/sistemi operativi potenzialmente interi.

L'astrazione risultante è costituita da due elementi di base. Il primo è il dizionario dell'ambiente. Questa struttura di dati è responsabile dell'archiviazione di tutti gli stati necessari per l'elaborazione di una richiesta e una risposta HTTP, nonché di qualsiasi stato del server pertinente. Il dizionario dell'ambiente viene definito come segue:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Un server Web compatibile con OWIN è responsabile della compilazione del dizionario dell'ambiente con dati quali i flussi del corpo e le raccolte di intestazioni per una richiesta e una risposta HTTP. È quindi responsabilità dei componenti dell'applicazione o del Framework compilare o aggiornare il dizionario con valori aggiuntivi e scrivere nel flusso del corpo della risposta.

Oltre a specificare il tipo per il dizionario dell'ambiente, la specifica OWIN definisce un elenco di coppie chiave-valore di base del dizionario. Ad esempio, nella tabella seguente vengono illustrate le chiavi del dizionario richieste per una richiesta HTTP:

| Nome della chiave | Descrizione del valore |
| --- | --- |
| `"owin.RequestBody"` | Flusso con il corpo della richiesta, se presente. Stream. null può essere usato come segnaposto se non è presente alcun corpo della richiesta. Vedere il [corpo della richiesta](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>` delle intestazioni della richiesta. Vedere [intestazioni](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | `string` contenente il metodo di richiesta HTTP della richiesta, ad esempio `"GET"`, `"POST"`. |
| `"owin.RequestPath"` | `string` contenente il percorso della richiesta. Il percorso deve essere relativo alla "radice" del delegato dell'applicazione. vedere [percorsi](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | `string` contenente la parte del percorso della richiesta corrispondente alla "radice" del delegato dell'applicazione. vedere [percorsi](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | `string` contenente il nome e la versione del protocollo (ad esempio `"HTTP/1.0"` o `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | `string` contenente il componente della stringa di query dell'URI della richiesta HTTP, senza il carattere "?" (ad esempio, `"foo=bar&baz=quux"`). Il valore può essere una stringa vuota. |
| `"owin.RequestScheme"` | `string` contenente lo schema URI utilizzato per la richiesta, ad esempio `"http"`, `"https"`. vedere [schema URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

Il secondo elemento chiave di OWIN è il delegato dell'applicazione. Si tratta di una firma di funzione che funge da interfaccia principale tra tutti i componenti di un'applicazione OWIN. La definizione del delegato dell'applicazione è la seguente:

`Func<IDictionary<string, object>, Task>;`

Il delegato dell'applicazione è quindi semplicemente un'implementazione del tipo delegato Func in cui la funzione accetta il dizionario dell'ambiente come input e restituisce un'attività. Questa progettazione presenta diverse implicazioni per gli sviluppatori:

- Per scrivere i componenti OWIN, è necessario un numero molto ridotto di dipendenze di tipo. Questo aumenta significativamente l'accessibilità di OWIN agli sviluppatori.
- La progettazione asincrona consente all'astrazione di essere efficiente con la gestione delle risorse di elaborazione, in particolare in un numero maggiore di operazioni di I/O intensive.
- Poiché il delegato dell'applicazione è un'unità di esecuzione atomica e perché il dizionario dell'ambiente viene portato come parametro nel delegato, i componenti OWIN possono essere concatenati facilmente per creare pipeline di elaborazione HTTP complesse.

Dal punto di vista dell'implementazione, OWIN è una specifica ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Il suo obiettivo non è quello di essere il framework Web successivo, bensì una specifica per interagire con i framework Web e i server Web.

Se è stato esaminato [OWIN](http://owin.org/) o [Katana](https://github.com/aspnet/AspNetKatana/wiki), è possibile che si sia notato anche il [pacchetto NuGet OWIN](http://nuget.org/packages/Owin) e OWIN. dll. Questa libreria contiene una singola interfaccia, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), che formalizza e codifica la sequenza di avvio descritta nella [sezione 4](http://owin.org/html/owin.html#4-application-startup) della specifica OWIN. Sebbene non sia necessario per compilare server OWIN, l'interfaccia [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) fornisce un punto di riferimento concreto e viene usata dai componenti del progetto Katana.

## <a name="project-katana"></a>Progetto Katana

Mentre sia la specifica [OWIN](http://owin.org/html/owin.html) che *OWIN. dll* sono di proprietà della community e la community esegue attività open source, il progetto [Katana](https://github.com/aspnet/AspNetKatana/wiki) rappresenta il set di componenti OWIN che, mentre ancora open source, vengono creati e rilasciati da Microsoft. Questi componenti includono entrambi i componenti dell'infrastruttura, ad esempio gli host e i server, nonché i componenti funzionali, ad esempio i componenti di autenticazione e le associazioni a Framework come [SignalR](../../../signalr/index.md) e [API Web ASP.NET](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Il progetto presenta i tre obiettivi di alto livello seguenti: 

- **Portabile: i** componenti devono poter essere facilmente sostituiti per i nuovi componenti man mano che diventano disponibili. Sono inclusi tutti i tipi di componenti, dal Framework al server e all'host. L'implicazione di questo obiettivo è che i Framework di terze parti possono essere eseguiti facilmente sui server Microsoft, mentre i Framework Microsoft possono essere potenzialmente eseguiti su server e host di terze parti.
- **Modulare/flessibile**: a differenza di molti Framework che includono una miriade di funzionalità attivate per impostazione predefinita, i componenti del progetto Katana dovrebbero essere piccoli e mirati, assicurando al contempo il controllo dello sviluppatore dell'applicazione per determinare quali componenti usare nell'applicazione.
- **Lightweight/performanceable/Scalable** : suddividendo la nozione tradizionale di un Framework in un set di piccoli componenti mirati che vengono aggiunti in modo esplicito dallo sviluppatore di applicazioni, un'applicazione Katana risultante può utilizzare un minor numero di risorse di calcolo e, di conseguenza, gestire un carico maggiore rispetto ad altri tipi di server e Framework. Poiché i requisiti dell'applicazione richiedono più funzionalità dall'infrastruttura sottostante, è possibile aggiungerle alla pipeline OWIN, ma questo dovrebbe essere una decisione esplicita da parte dello sviluppatore di applicazioni. Inoltre, la sostituzione dei componenti di livello inferiore significa che, man mano che diventano disponibili, i nuovi server a prestazioni elevate possono essere introdotti senza interruzioni per migliorare le prestazioni delle applicazioni OWIN senza suddividere tali applicazioni.

## <a name="getting-started-with-katana-components"></a>Introduzione con i componenti Katana

Quando è stata introdotta per la prima volta, un aspetto del Framework [node. js](http://nodejs.org/) che ha immediatamente attirato l'attenzione degli utenti è la semplicità con cui è possibile creare ed eseguire un server Web. Se gli obiettivi di Katana erano incorniciati alla luce di [node. js](http://nodejs.org/), è possibile riepilogarli affermando che Katana offre molti dei vantaggi di [node. js](http://nodejs.org/) (e di Framework come it) senza forzare lo sviluppatore a generare tutto ciò che sa sullo sviluppo di applicazioni Web ASP.NET. Per tenere presente questa istruzione, iniziare a usare il progetto Katana dovrebbe essere ugualmente semplice per [node. js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Creazione di "Hello World!"

Una differenza rilevante tra lo sviluppo in JavaScript e .NET è la presenza o l'assenza di un compilatore. Per questo motivo, il punto di partenza per un semplice server Katana è un progetto di Visual Studio. Tuttavia, è possibile iniziare con i tipi di progetto più minimi: l'applicazione Web ASP.NET vuota.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Si installerà quindi il pacchetto NuGet [Microsoft. Owin. host. systemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) nel progetto. Questo pacchetto fornisce un server OWIN che viene eseguito nella pipeline della richiesta ASP.NET. Si trova nella [raccolta NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) e può essere installato usando la finestra di dialogo Gestione pacchetti di Visual Studio o la console di gestione pacchetti con il comando seguente:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

L'installazione del pacchetto di `Microsoft.Owin.Host.SystemWeb` installerà alcuni pacchetti aggiuntivi come dipendenze. Una di queste dipendenze è `Microsoft.Owin`, una libreria che fornisce diversi tipi di helper e metodi per lo sviluppo di applicazioni OWIN. È possibile usare questi tipi per scrivere rapidamente il seguente server "Hello World".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Questo server Web molto semplice può ora essere eseguito con il comando **F5** di Visual Studio e include il supporto completo per il debug.

## <a name="switching-hosts"></a>Cambio host

Per impostazione predefinita, l'esempio precedente "Hello World" viene eseguito nella pipeline delle richieste ASP.NET, che usa System. Web nel contesto di IIS. Questo può aggiungere un notevole valore perché consente di trarre vantaggio dalla flessibilità e dalla composizione di una pipeline OWIN con le funzionalità di gestione e la maturità complessiva di IIS. Tuttavia, in alcuni casi i vantaggi offerti da IIS non sono obbligatori e il desiderio è quello di un host più piccolo e leggero. Quali sono gli elementi necessari per eseguire il server Web semplice all'esterno di IIS e System. Web?

Per illustrare l'obiettivo della portabilità, il passaggio da un host server Web a un host della riga di comando richiede semplicemente l'aggiunta del nuovo server e delle dipendenze host alla cartella di output del progetto e l'avvio dell'host. In questo esempio, il server Web verrà ospitato in un host Katana denominato `OwinHost.exe` e utilizzerà il server di Katana basato su HttpListener. Analogamente agli altri componenti Katana, questi verranno acquisiti da NuGet usando il comando seguente:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Dalla riga di comando è possibile passare alla cartella radice del progetto ed eseguire semplicemente il `OwinHost.exe`, che è stato installato nella cartella strumenti del rispettivo pacchetto NuGet. Per impostazione predefinita, `OwinHost.exe` è configurato per la ricerca del server basato su HttpListener e pertanto non è necessaria alcuna configurazione aggiuntiva. Se si passa a un Web browser `http://localhost:5000/` viene visualizzata l'applicazione che ora viene eseguita tramite la console.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Architettura Katana

 L'architettura del componente Katana divide un'applicazione in quattro livelli logici, come illustrato di seguito: *host, server, middleware* e *Application*. L'architettura del componente viene fattorizzata in modo tale che le implementazioni di questi livelli possano essere facilmente sostituite, in molti casi, senza richiedere la ricompilazione dell'applicazione.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 L'host è responsabile di:

- Gestione del processo sottostante.
- Orchestrazione del flusso di lavoro che comporta la selezione di un server e la costruzione di una pipeline OWIN tramite cui verranno gestite le richieste.

  Attualmente sono disponibili tre opzioni di hosting principali per le applicazioni basate su Katana:  
  
**IIS/ASP. NET**: usando i tipi HttpModule e HttpHandler standard, le pipeline OWIN possono essere eseguite in IIS come parte di un flusso di richiesta ASP.NET. Il supporto per l'hosting di ASP.NET viene abilitato installando il pacchetto NuGet Microsoft. AspNet. host. SystemWeb in un progetto di applicazione Web. Poiché IIS funge sia da host che da server, inoltre, la distinzione tra server e host OWIN è conforme al pacchetto NuGet, ovvero se si utilizza l'host SystemWeb, uno sviluppatore non può sostituire un'implementazione del server alternativa.  
  
**Host personalizzato**: la suite di componenti Katana offre agli sviluppatori la possibilità di ospitare applicazioni nel proprio processo personalizzato, sia che si tratti di un'applicazione console, di un servizio Windows e così via. Questa funzionalità ha un aspetto simile alla funzionalità self-host fornita dall'API Web. L'esempio seguente mostra un host personalizzato di codice API Web:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

La configurazione Self-host per un'applicazione Katana è simile:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Una differenza importante tra gli esempi di API Web e Katana Self-host è che il codice di configurazione dell'API Web non è presente nell'esempio della katana Self-host. Per consentire la portabilità e la composizione, Katana separa il codice che avvia il server dal codice che configura la pipeline di elaborazione delle richieste. Il codice che configura l'API Web, quindi è contenuto nell'avvio della classe, che viene inoltre specificato come parametro di tipo in WebApplication. Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

La classe Startup verrà discussa più dettagliatamente più avanti in questo articolo. Tuttavia, il codice necessario per avviare un processo self-host Katana sembra sorprendentemente simile al codice che è possibile utilizzare oggi in API Web ASP.NET applicazioni self-host.

**OwinHost. exe**: Sebbene alcuni desiderino scrivere un processo personalizzato per l'esecuzione di applicazioni Web Katana, molti preferiscono semplicemente avviare un eseguibile predefinito in grado di avviare un server ed eseguire la propria applicazione. Per questo scenario, il gruppo di componenti Katana include `OwinHost.exe`. Quando viene eseguito dall'interno della directory radice di un progetto, questo eseguibile avvia un server (per impostazione predefinita usa il server HttpListener) e usa le convenzioni per trovare ed eseguire la classe di avvio dell'utente. Per un controllo più granulare, il file eseguibile fornisce numerosi parametri della riga di comando aggiuntivi.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Server

 Mentre l'host è responsabile dell'avvio e della gestione del processo all'interno del quale viene eseguita l'applicazione, la responsabilità del server consiste nell'aprire un socket di rete, restare in ascolto delle richieste e inviarli tramite la pipeline dei componenti OWIN specificati dall'utente (come si è possibile che sia già stato notato che questa pipeline è specificata nella classe startup dello sviluppatore dell'applicazione. Attualmente, il progetto Katana include due implementazioni server: 

- **Microsoft. Owin. host. systemWeb**: come indicato in precedenza, IIS in concerto con la pipeline ASP.NET funge sia da host che da server. Pertanto, quando si sceglie questa opzione di hosting, IIS gestisce entrambe le problematiche a livello di host, ad esempio l'attivazione del processo e l'ascolto delle richieste HTTP. Per le applicazioni Web ASP.NET, invia le richieste alla pipeline ASP.NET. L'host SystemWeb Katana registra un ASP.NET HttpModule e HttpHandler per intercettare le richieste mentre passano attraverso la pipeline HTTP e inviarle tramite la pipeline OWIN specificata dall'utente.
- **Microsoft. Owin. host. HttpListener**: come indicato dal nome, questo server Katana usa la classe HttpListener del .NET Framework per aprire un socket e inviare richieste in una pipeline Owin specificata dallo sviluppatore. Si tratta attualmente della selezione server predefinita per l'API self-host Katana e per OwinHost. exe.

## <a name="middlewareframework"></a>Middleware/Framework

 Come indicato in precedenza, quando il server accetta una richiesta da un client, è responsabile di passarla tramite una pipeline di componenti OWIN, che sono specificati dal codice di avvio dello sviluppatore. Questi componenti della pipeline sono noti come middleware.  
 A un livello molto semplice, un componente middleware OWIN deve semplicemente implementare il delegato dell'applicazione OWIN in modo che sia richiamabile.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Tuttavia, per semplificare lo sviluppo e la composizione di componenti middleware, Katana supporta una serie di convenzioni e tipi di helper per i componenti middleware. Il più comune di questi è la classe `OwinMiddleware`. Un componente middleware personalizzato compilato con questa classe avrà un aspetto simile al seguente: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Questa classe deriva da `OwinMiddleware`, implementa un costruttore che accetta un'istanza del middleware successivo nella pipeline come uno degli argomenti e quindi lo passa al costruttore di base. Gli argomenti aggiuntivi usati per configurare il middleware vengono anche dichiarati come parametri del costruttore dopo il parametro middleware successivo.   
  
In fase di esecuzione, il middleware viene eseguito tramite il metodo di `Invoke` sottoposto a override. Questo metodo accetta un solo argomento di tipo `OwinContext`. Questo oggetto di contesto viene fornito dal pacchetto NuGet `Microsoft.Owin` descritto in precedenza e fornisce l'accesso fortemente tipizzato alla richiesta, alla risposta e al dizionario dell'ambiente, oltre ad alcuni tipi di helper aggiuntivi.   
  
La classe middleware può essere facilmente aggiunta alla pipeline OWIN nel codice di avvio dell'applicazione, come indicato di seguito:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Poiché l'infrastruttura Katana crea semplicemente una pipeline di componenti middleware OWIN e, poiché i componenti devono semplicemente supportare il delegato dell'applicazione per partecipare alla pipeline, i componenti del middleware possono variare in modo complesso da logger semplici a Framework interi come ASP.NET, API Web o [SignalR](../../../signalr/index.md). Ad esempio, per aggiungere API Web ASP.NET alla pipeline OWIN precedente è necessario aggiungere il codice di avvio seguente:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

L'infrastruttura Katana compilerà la pipeline dei componenti del middleware in base all'ordine in cui sono stati aggiunti all'oggetto IAppBuilder nel metodo di configurazione. In questo esempio, LoggerMiddleware è in grado di gestire tutte le richieste che passano attraverso la pipeline, indipendentemente dal modo in cui tali richieste vengono gestite. Questo consente scenari avanzati in cui un componente middleware, ad esempio un componente di autenticazione, è in grado di elaborare le richieste per una pipeline che include più componenti e Framework, ad esempio API Web ASP.NET, SignalR e una file server statica.
 
## <a name="applications"></a>Applicazioni

Come illustrato negli esempi precedenti, OWIN e il progetto Katana non devono essere considerati come un nuovo modello di programmazione dell'applicazione, ma piuttosto come un'astrazione per separare i modelli e i Framework di programmazione delle applicazioni dall'infrastruttura server e host. Ad esempio, quando si compilano applicazioni API Web, il Framework per sviluppatori continuerà a usare il Framework di API Web ASP.NET, indipendentemente dal fatto che l'applicazione venga eseguita in una pipeline OWIN usando i componenti del progetto Katana. L'unica posizione in cui il codice correlato a OWIN sarà visibile allo sviluppatore di applicazioni sarà il codice di avvio dell'applicazione, in cui lo sviluppatore compone la pipeline OWIN. Nel codice di avvio lo sviluppatore registrerà una serie di istruzioni UseXx, in genere una per ogni componente middleware che elaborerà le richieste in ingresso. Questa esperienza avrà lo stesso effetto della registrazione dei moduli HTTP nell'attuale System. Web World. In genere, un middleware di Framework più ampio, ad esempio API Web ASP.NET o [SignalR](../../../signalr/index.md) , verrà registrato alla fine della pipeline. I componenti middleware trasversali, ad esempio quelli per l'autenticazione o la memorizzazione nella cache, vengono in genere registrati verso l'inizio della pipeline in modo che elaborino le richieste per tutti i Framework e i componenti registrati in un secondo momento nella pipeline. Questa separazione dei componenti del middleware tra loro e dai componenti dell'infrastruttura sottostante consente ai componenti di evolversi a velocità diverse garantendo al contempo che il sistema globale rimanga stabile.

## <a name="components--nuget-packages"></a>Componenti-pacchetti NuGet

Analogamente a molte librerie e Framework correnti, i componenti del progetto Katana vengono recapitati come un set di pacchetti NuGet. Per la versione futura 2,0, il grafico delle dipendenze del pacchetto Katana è simile al seguente. (Fare clic sull'immagine per visualizzare le dimensioni maggiori).

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Quasi tutti i pacchetti nel progetto Katana dipendono, direttamente o indirettamente, nel pacchetto Owin. È possibile ricordare che si tratta del pacchetto che contiene l'interfaccia IAppBuilder, che fornisce un'implementazione concreta della sequenza di avvio dell'applicazione descritta nella sezione 4 della specifica OWIN. Inoltre, molti dei pacchetti dipendono da Microsoft. Owin, che fornisce un set di tipi helper per l'utilizzo di richieste e risposte HTTP. Il resto del pacchetto può essere classificato come pacchetti di infrastruttura host (server o host) o middleware. I pacchetti e le dipendenze esterne al progetto Katana vengono visualizzati in arancione.

L'infrastruttura host per Katana 2,0 include i server basati su SystemWeb e HttpListener, il pacchetto OwinHost per l'esecuzione di applicazioni OWIN con OwinHost. exe e il pacchetto Microsoft. Owin. hosting per le applicazioni OWIN self-hosting in una host personalizzato, ad esempio applicazione console, servizio Windows e così via.

Per Katana 2,0, i componenti del middleware si concentrano principalmente sull'offerta di metodi di autenticazione diversi. Viene fornito un ulteriore componente middleware per la diagnostica, che Abilita il supporto per una pagina di avvio e di errore. Man mano che OWIN cresce nell'astrazione di hosting, l'ecosistema di componenti middleware, sia quelli sviluppati da Microsoft che di terze parti, aumenterà anche in numero.

## <a name="conclusion"></a>Conclusione

 Dall'inizio, l'obiettivo del progetto Katana non è stato quello di creare e quindi forzare gli sviluppatori ad apprendere un altro framework Web. L'obiettivo è invece quello di creare un'astrazione per offrire agli sviluppatori di applicazioni Web .NET una scelta più ampia rispetto a quella già disponibile. Suddividendo i livelli logici di un tipico stack di applicazioni Web in un set di componenti sostituibili, il progetto Katana consente ai componenti di tutto lo stack di migliorare con qualsiasi frequenza per tali componenti. Costruendo tutti i componenti intorno alla semplice astrazione OWIN, Katana consente ai Framework e alle applicazioni basate su di essi di essere portabili in un'ampia gamma di server e host diversi. Mettendo lo sviluppatore a controllare lo stack, Katana garantisce che lo sviluppatore sia la scelta ideale per la leggerezza o il modo in cui il suo stack Web è ricco di funzionalità.  

## <a name="for-more-information-about-katana"></a>Per altre informazioni su Katana

- Il progetto Katana su GitHub: [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/).
- Video: [il progetto Katana-OWIN per ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), di Howard Dierking.

## <a name="acknowledgements"></a>Riconoscimenti

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT) ) Rick è Senior Programming Writer per Microsoft focalizzato su Azure e MVC.
- [Scott hanseln](http://www.hanselman.com/blog/): (Twitter [@shanselman](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (Twitter [@jongalloway](https://twitter.com/jongalloway) )
