---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: Uso di metodi asincroni in ASP.NET 4,5 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra le nozioni di base per la creazione di un'applicazione Web Forms ASP.NET asincrona usando Visual Studio Express 2012 per il Web, che è una versione gratuita...
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 7abc3d7acc60d7d868958f2a313bc408f96c95a4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457570"
---
# <a name="using-asynchronous-methods-in-aspnet-45"></a>Uso di metodi asincroni in ASP.NET 4.5

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Questa esercitazione illustra le nozioni di base della creazione di un'applicazione Web Forms ASP.NET asincrona usando [Visual Studio Express 2012 per il Web](https://www.microsoft.com/visualstudio/11), una versione gratuita di Microsoft Visual Studio. È anche possibile usare [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). In questa esercitazione sono incluse le sezioni seguenti.
> 
> - [Modalità di elaborazione delle richieste da parte del pool di thread](#HowRequestsProcessedByTP)
> - [Scelta di metodi sincroni o asincroni](#ChoosingSyncVasync)
> - [Applicazione di esempio](#SampleApp)
> - [Pagina sincrona dei gizmo](#GizmosSynch)
> - [Creazione di una pagina di Gizmo asincrona](#CreatingAsynchGizmos)
> - [Esecuzione di più operazioni in parallelo](#Parallel)
> - [Uso di un token di annullamento](#CancelToken)
> - [Configurazione del server per le chiamate al servizio Web a concorrenza elevata e a latenza elevata](#ServerConfig)
> 
> Per questa esercitazione è disponibile un esempio completo  
> [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) nel sito [GitHub](https://github.com/) .

ASP.NET 4,5 le pagine Web in combinazione con [.net 4,5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) consentono di registrare metodi asincroni che restituiscono un oggetto di tipo [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Il .NET Framework 4 ha introdotto un concetto di programmazione asincrona definito [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) e ASP.NET 4,5 supporta l' [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Le attività sono rappresentate dal tipo di **attività** e dai tipi correlati nello spazio dei nomi [System. Threading. Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) . Il .NET Framework 4,5 si basa su questo supporto asincrono con le parole chiave [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) che semplificano l'utilizzo di oggetti [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) molto meno complessi rispetto agli approcci asincroni precedenti. La parola chiave [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) è un'abbreviazione sintattica per indicare che un frammento di codice deve attendere in modo asincrono un altro frammento di codice. La parola chiave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) rappresenta un hint che è possibile usare per contrassegnare i metodi come metodi asincroni basati su attività. La combinazione di **await**, **Async**e dell'oggetto **attività** rende molto più semplice scrivere codice asincrono in .NET 4,5. Il nuovo modello per i metodi asincroni è denominato *modello asincrono basato su attività* (**Tap**). In questa esercitazione si presuppone una certa familiarità con la programmazione asincrona utilizzando le parole chiave [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e lo spazio dei nomi dell' [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) .

Per ulteriori informazioni sull'utilizzo delle parole chiave [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e dello spazio dei nomi [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , vedere i riferimenti seguenti.

- [White paper: modalità asincrona in .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Domande frequenti su async/await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programmazione asincrona di Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Modalità di elaborazione delle richieste da parte del pool di thread

Sul server Web, il .NET Framework gestisce un pool di thread utilizzati per soddisfare le richieste ASP.NET. Quando arriva una richiesta, viene inviato un thread dal pool per elaborarla. Se la richiesta viene elaborata in modo sincrono, il thread che elabora la richiesta è occupato durante l'elaborazione della richiesta e tale thread non può soddisfare un'altra richiesta.   
  
Questo potrebbe non essere un problema, perché il pool di thread può essere sufficientemente grande da contenere molti thread occupati. Tuttavia, il numero di thread nel pool di thread è limitato (il valore massimo predefinito per .NET 4,5 è 5.000). Nelle applicazioni di grandi dimensioni con concorrenza elevata di richieste con esecuzione prolungata, tutti i thread disponibili potrebbero essere occupati. Questa condizione è nota come mancanza di risorse dei thread. Quando viene raggiunta questa condizione, il server Web esegue la coda delle richieste. Se la coda di richieste diventa piena, il server Web rifiuta le richieste con stato HTTP 503 (server troppo occupato). Il pool di thread CLR presenta limitazioni per i nuovi Injection thread. Se la concorrenza è di gran lunga (ovvero, il sito Web può improvvisamente ottenere un numero elevato di richieste) e tutti i thread di richiesta disponibili sono occupati a causa di chiamate back-end con latenza elevata, la velocità di inserimento dei thread limitata può rendere la risposta dell'applicazione molto scarsa. Inoltre, ogni nuovo thread aggiunto al pool di thread presenta un sovraccarico (ad esempio 1 MB di memoria dello stack). Un'applicazione Web che utilizza metodi sincroni per gestire chiamate a latenza elevata in cui il pool di thread raggiunge il valore massimo predefinito di .NET 4,5 di 5 000 thread utilizzerebbe circa 5 GB di memoria superiore a quella che un'applicazione può richiedere al servizio le stesse richieste usando metodi asincroni e solo 50 thread. Quando si esegue il lavoro asincrono, non sempre si usa un thread. Ad esempio, quando si esegue una richiesta asincrona di servizio Web, ASP.NET non utilizzerà alcun thread tra la chiamata **al metodo asincrono e l'** **attesa**. L'uso del pool di thread per soddisfare le richieste con latenza elevata può causare un footprint di memoria di grandi dimensioni e un utilizzo scarso dell'hardware del server.

## <a name="processing-asynchronous-requests"></a>Elaborazione di richieste asincrone

Nelle applicazioni Web che visualizzano un numero elevato di richieste simultanee all'avvio o con un carico eccessivo (in caso di aumento improvviso della concorrenza), il fatto che il servizio Web chiami asincrono aumenterà la velocità di risposta dell'applicazione. L'elaborazione di una richiesta asincrona ha la stessa durata di una richiesta sincrona. Se, ad esempio, una richiesta effettua una chiamata a un servizio Web che richiede due secondi per il completamento, la richiesta richiede due secondi se viene eseguita in modo sincrono o asincrono. Tuttavia, durante una chiamata asincrona, un thread non viene bloccato dalla risposta ad altre richieste durante l'attesa del completamento della prima richiesta. Pertanto, le richieste asincrone impediscono l'accodamento delle richieste e la crescita del pool di thread quando sono presenti molte richieste simultanee che richiamano operazioni a esecuzione prolungata

## <a id="ChoosingSyncVasync"></a>Scelta di metodi sincroni o asincroni

In questa sezione vengono elencate le linee guida per l'utilizzo dei metodi sincroni o asincroni. Si tratta solo di linee guida; esaminare ogni singola applicazione per determinare se i metodi asincroni consentono di migliorare le prestazioni.

In generale, usare i metodi sincroni per le condizioni seguenti:

- Le operazioni sono semplici o a esecuzione breve.
- La semplicità è più importante dell'efficienza.
- Le operazioni sono principalmente operazioni della CPU anziché operazioni che comportano un sovraccarico esteso del disco o della rete. L'utilizzo di metodi asincroni sulle operazioni con associazione alla CPU non offre alcun vantaggio e comporta un sovraccarico.

In generale, usare i metodi asincroni per le condizioni seguenti:

- Si stanno chiamando servizi che possono essere usati tramite metodi asincroni e si usa .NET 4,5 o versione successiva.
- Le operazioni sono associate alla rete o con vincoli di I/O anziché essere associate alla CPU.
- Il parallelismo è più importante della semplicità del codice.
- Si desidera fornire un meccanismo che consente agli utenti di annullare una richiesta a esecuzione prolungata.
- Quando il vantaggio del cambio di thread supera il costo del cambio di contesto. In generale, è consigliabile rendere asincrono un metodo se il metodo sincrono blocca il thread di richiesta ASP.NET senza eseguire alcuna operazione. Rendendo asincrona la chiamata, il thread di richiesta ASP.NET non viene bloccato senza alcun intervento durante l'attesa del completamento della richiesta del servizio Web.
- Il test indica che le operazioni di blocco costituiscono un collo di bottiglia nelle prestazioni del sito e che IIS può servire più richieste usando metodi asincroni per queste chiamate di blocco.

  Nell'esempio scaricabile viene illustrato come utilizzare i metodi asincroni in modo efficace. L'esempio fornito è stato progettato per fornire una semplice dimostrazione della programmazione asincrona in ASP.NET 4,5. L'esempio non è destinato a essere un'architettura di riferimento per la programmazione asincrona in ASP.NET. Il programma di esempio chiama [API Web ASP.NET](../../../web-api/index.md) metodi che a loro volta chiamano [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) per simulare le chiamate al servizio Web con esecuzione prolungata. Nella maggior parte delle applicazioni di produzione non vengono mostrati i vantaggi evidenti dell'utilizzo di metodi asincroni.   
  
Per alcune applicazioni è necessario che tutti i metodi siano asincroni. Spesso, la conversione di alcuni metodi sincroni in metodi asincroni fornisce il migliore miglioramento dell'efficienza per la quantità di lavoro necessaria.

## <a id="SampleApp"></a>Applicazione di esempio

È possibile scaricare l'applicazione di esempio da [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) nel sito [GitHub](https://github.com/) . Il repository è costituito da tre progetti:

- *WebAppAsync*: progetto web form ASP.NET che usa il servizio **WebAPIpwg** dell'API Web. La maggior parte del codice per questa esercitazione è del progetto.
- *WebAPIpgw*: progetto API Web ASP.NET MVC 4 che implementa i controller di `Products, Gizmos and Widgets`. Fornisce i dati per il progetto *WebAppAsync* e il progetto *Mvc4Async* .
- *Mvc4Async*: il progetto ASP.NET MVC 4 che contiene il codice usato in un'altra esercitazione. Esegue chiamate API Web al servizio **WebAPIpwg** .

## <a id="GizmosSynch"></a>Pagina sincrona dei gizmo

 Il codice seguente illustra il metodo sincrono `Page_Load` usato per visualizzare un elenco di Gizmo. Per questo articolo, un Gizmo è un dispositivo meccanico fittizio. 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Il codice seguente illustra il metodo `GetGizmos` del servizio Gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

Il metodo `GizmoService GetGizmos` passa un URI a un servizio HTTP API Web ASP.NET che restituisce un elenco di dati dei gizmo. Il progetto *WebAPIpgw* contiene l'implementazione del `gizmos, widget` dell'API Web e dei controller `product`.  
La figura seguente mostra la pagina dei gizmo dal progetto di esempio.

![Gizmo](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Creazione di una pagina di Gizmo asincrona

Nell'esempio vengono utilizzate le nuove parole chiave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) (disponibili in .NET 4,5 e Visual Studio 2012) per consentire al compilatore di gestire le trasformazioni complesse necessarie per la programmazione asincrona. Il compilatore consente di scrivere codice usando i C#costrutti del flusso di controllo sincroni e il compilatore applica automaticamente le trasformazioni necessarie per usare i callback per evitare il blocco dei thread.

Le pagine asincrone ASP.NET devono includere la direttiva della [pagina](https://msdn.microsoft.com/library/ydy4x04a.aspx) con l'attributo `Async` impostato su "true". Nel codice seguente viene illustrata la direttiva della [pagina](https://msdn.microsoft.com/library/ydy4x04a.aspx) con l'attributo `Async` impostato su "true" per la pagina *GizmosAsync. aspx* .

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Il codice seguente illustra il metodo di `Page_Load` sincrono `Gizmos` e la pagina asincrona `GizmosAsync`. Se il browser supporta l' [elemento HTML 5 &lt;mark&gt;](http://www.w3.org/wiki/HTML/Elements/mark), verranno visualizzate le modifiche in `GizmosAsync` evidenziate in giallo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

Versione asincrona:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Sono state applicate le seguenti modifiche per consentire al `GizmosAsync` pagina di essere asincrona.

- Per la direttiva della [pagina](https://msdn.microsoft.com/library/ydy4x04a.aspx) è necessario che l'attributo `Async` sia impostato su "true".
- Il metodo `RegisterAsyncTask` viene usato per registrare un'attività asincrona contenente il codice che viene eseguito in modo asincrono.
- Il nuovo metodo `GetGizmosSvcAsync` è contrassegnato con la parola chiave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , che indica al compilatore di generare callback per parti del corpo e creare automaticamente un `Task` restituito.
- &quot;&quot; Async è stato aggiunto al nome del metodo asincrono. L'accodamento di "Async" non è obbligatorio, ma è la convenzione durante la scrittura di metodi asincroni.
- Il tipo restituito del nuovo metodo di `GetGizmosSvcAsync` è `Task`. Il tipo restituito di `Task` rappresenta il lavoro in corso e fornisce ai chiamanti del metodo un handle tramite il quale attendere il completamento dell'operazione asincrona.
- La parola chiave [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) è stata applicata alla chiamata al servizio Web.
- È stata chiamata l'API del servizio Web asincrona (`GetGizmosAsync`).

All'interno del corpo del metodo `GetGizmosSvcAsync` viene chiamato un altro metodo asincrono, `GetGizmosAsync`. `GetGizmosAsync` restituisce immediatamente un `Task<List<Gizmo>>` che verrà completato quando i dati sono disponibili. Poiché non si desidera eseguire altre operazioni fino a quando non si dispone dei dati del gizmo, il codice attende l'attività (usando la parola chiave **await** ). È possibile usare la parola chiave **await** solo nei metodi annotati con la parola chiave **Async** .

La parola chiave **await** non blocca il thread finché l'attività non viene completata. Registra il resto del metodo come callback nell'attività e restituisce immediatamente. Quando l'attività attesa viene completata, richiama il callback e quindi riprende l'esecuzione del metodo direttamente da dove era stato interrotto. Per ulteriori informazioni sull'utilizzo delle parole chiave [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e dello spazio dei nomi dell' [attività](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , vedere i [riferimenti asincroni](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Il codice seguente mostra i metodi `GetGizmos` e `GetGizmosAsync`.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Le modifiche asincrone sono simili a quelle apportate ai **GizmosAsync** precedenti. 

- La firma del metodo è stata annotata con la parola chiave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , il tipo restituito è stato modificato in `Task<List<Gizmo>>`e *Async* è stato aggiunto al nome del metodo.
- Viene utilizzata la classe asincrona [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) anziché la classe [client WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) sincrona.
- La parola chiave [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) è stata applicata al metodo asincrono [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) .

La figura seguente mostra la visualizzazione Gizmo asincrona.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

La presentazione dei dati dei gizmo nei browser è identica alla visualizzazione creata dalla chiamata sincrona. L'unica differenza è che la versione asincrona può essere più efficiente in carichi pesanti.

## <a name="registerasynctask-notes"></a>Note su RegisterAsyncTask

I metodi collegati con `RegisterAsyncTask` vengono eseguiti immediatamente dopo [il prerendering](https://msdn.microsoft.com/library/ms178472.aspx). È anche possibile usare direttamente eventi di pagine void void, come illustrato nel codice seguente:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Lo svantaggio degli eventi void asincroni è che gli sviluppatori non hanno più il controllo completo su quando vengono eseguiti gli eventi. Ad esempio, se sia un. aspx che un. Il master definisce `Page_Load` eventi e uno o entrambi sono asincroni, non è possibile garantire l'ordine di esecuzione. Viene applicato lo stesso ordine indeterminiate per i gestori non di eventi (ad esempio `async void Button_Click`). Per la maggior parte degli sviluppatori questo dovrebbe essere accettabile, ma gli utenti che richiedono il controllo completo sull'ordine di esecuzione devono usare solo API come `RegisterAsyncTask` che utilizzano metodi che restituiscono un oggetto attività.

## <a id="Parallel"></a>Esecuzione di più operazioni in parallelo

I metodi asincroni presentano un vantaggio significativo rispetto ai metodi sincroni quando un'azione deve eseguire diverse operazioni indipendenti. Nell'esempio fornito, la pagina sincrona *PWG. aspx*(per prodotti, widget e Gizmo) Visualizza i risultati di tre chiamate al servizio Web per ottenere un elenco di prodotti, widget e Gizmo. Il progetto [API Web ASP.NET](../../../web-api/index.md) che fornisce questi servizi utilizza [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) per simulare la latenza o le chiamate di rete lente. Quando il ritardo è impostato su 500 millisecondi, la pagina *PWGasync. aspx* asincrona impiega un po' di 500 millisecondi per il completamento mentre la versione sincrona `PWG` richiede più di 1.500 millisecondi. Il codice seguente illustra la pagina *PWG. aspx* sincrona.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Di seguito è riportato il codice di `PWGasync` asincrono.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

Nell'immagine seguente viene illustrata la vista restituita dalla pagina *PWGasync. aspx* asincrona.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Uso di un token di annullamento

I metodi asincroni che restituiscono `Task`sono annullabili, ovvero accettano un parametro [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) quando ne viene fornito uno con l'attributo `AsyncTimeout` della direttiva della [pagina](https://msdn.microsoft.com/library/ydy4x04a.aspx) . Il codice seguente illustra la pagina *GizmosCancelAsync. aspx* con un timeout di sul secondo.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Il codice seguente illustra il file *GizmosCancelAsync.aspx.cs* .

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

Nell'applicazione di esempio fornita, selezionando il collegamento *GizmosCancelAsync* viene chiamata la pagina *GizmosCancelAsync. aspx* e viene illustrato l'annullamento (mediante timeout) della chiamata asincrona. Poiché il tempo di ritardo rientra in un intervallo casuale, potrebbe essere necessario aggiornare la pagina un paio di volte per ottenere il messaggio di errore di timeout.

## <a id="ServerConfig"></a>Configurazione del server per le chiamate al servizio Web a concorrenza elevata e a latenza elevata

Per realizzare i vantaggi di un'applicazione Web asincrona, potrebbe essere necessario apportare alcune modifiche alla configurazione predefinita del server. Quando si configurano e si testa il test dell'applicazione Web asincrona, tenere presente quanto segue.

- Windows 7, Windows Vista, Window 8 e tutti i sistemi operativi client Windows hanno un massimo di 10 richieste simultanee. È necessario un sistema operativo Windows Server per vedere i vantaggi dei metodi asincroni in condizioni di carico elevato.
- Registrare .NET 4,5 con IIS da un prompt dei comandi con privilegi elevati usando il comando seguente:  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis-i  
  Vedere [ASP.NET IIS Registration Tool (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Potrebbe essere necessario aumentare il limite della coda [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) dal valore predefinito 1.000 a 5.000. Se l'impostazione è troppo bassa, è possibile che venga visualizzato [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) Rifiuta richieste con stato HTTP 503. Per modificare il limite della coda HTTP. sys:

    - Aprire Gestione IIS e passare al riquadro pool di applicazioni.
    - Fare clic con il pulsante destro del mouse sul pool di applicazioni di destinazione e scegliere **Impostazioni avanzate**.  
        ![](using-asynchronous-methods-in-aspnet-45/_static/image4.png) avanzate
    - Nella finestra di dialogo **Impostazioni avanzate** modificare la *lunghezza della coda* da 1.000 a 5.000.  
        Lunghezza coda ![](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  Nota nelle immagini precedenti, .NET Framework è elencato come v 4.0, anche se il pool di applicazioni USA .NET 4,5. Per comprendere questa discrepanza, vedere gli argomenti seguenti:

- [Controllo delle versioni di .NET e multitargeting: .NET 4,5 è un aggiornamento sul posto a .NET 4,0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
- [Come impostare un'applicazione IIS o AppPool per l'uso di ASP.NET 3,5 anziché 2,0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
- [Versioni e dipendenze di .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)

- Se l'applicazione usa servizi Web o System.NET per comunicare con un back-end su HTTP, potrebbe essere necessario aumentare l'elemento [connectionManagement/MaxConnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) . Per le applicazioni ASP.NET, questo è limitato dalla funzionalità di configurazione automatica a 12 volte il numero di CPU. Ciò significa che in un quad-proc è possibile avere al massimo 12 \* 4 = 48 connessioni simultanee a un endpoint IP. Poiché è associato a [AutoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), il modo più semplice per aumentare `maxconnection` in un'applicazione ASP.NET consiste nell'impostare [System .NET. ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) a livello di codice nel metodo from `Application_Start` nel file *Global. asax* . Per un esempio, vedere il download di esempio.
- In .NET 4,5, il valore predefinito di 5000 per [maxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) dovrebbe essere corretto.

## <a name="contributors"></a>Collaboratori

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei GE](https://blogs.msdn.com/b/hongmeig/)
