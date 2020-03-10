---
uid: signalr/overview/advanced/dependency-injection
title: Inserimento di dipendenze in SignalR | Microsoft Docs
author: bradygaster
description: Le versioni del software usate in questo argomento Visual Studio 2013 .NET 4,5 SignalR versione 2 versioni precedenti di questo argomento per informazioni sulle versioni precedenti di...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 52978b10b6c131ac8eff4535216cc60b43fdf3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537331"
---
# <a name="dependency-injection-in-signalr"></a>Inserimento delle dipendenze in SignalR

di [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software usate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

L'inserimento di dipendenze è un modo per rimuovere le dipendenze hardcoded tra gli oggetti, semplificando la sostituzione delle dipendenze di un oggetto, per il testing (mediante oggetti fittizi) o per modificare il comportamento in fase di esecuzione. Questa esercitazione illustra come eseguire l'inserimento delle dipendenze negli hub SignalR. Viene inoltre illustrato come utilizzare i contenitori IoC con SignalR. Un contenitore IoC è un framework generale per l'inserimento di dipendenze.

## <a name="what-is-dependency-injection"></a>Che cos'è l'inserimento delle dipendenze?

Ignorare questa sezione se si ha già familiarità con l'inserimento di dipendenze.

L' *inserimento di dipendenze* è uno schema in cui gli oggetti non sono responsabili della creazione di dipendenze. Di seguito è riportato un semplice esempio per motivare il. Si supponga di disporre di un oggetto che deve registrare i messaggi. È possibile definire un'interfaccia di registrazione:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Nell'oggetto è possibile creare un `ILogger` per registrare i messaggi:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Questa operazione funziona, ma non è la progettazione migliore. Se si desidera sostituire `FileLogger` con un'altra implementazione di `ILogger`, sarà necessario modificare `SomeComponent`. Supponendo che la maggior parte degli altri oggetti usi `FileLogger`, sarà necessario modificarli tutti. In alternativa, se si decide di creare `FileLogger` un singleton, sarà anche necessario apportare modifiche all'interno dell'applicazione.

Un approccio migliore consiste nel "inserire" un `ILogger` nell'oggetto, ad esempio usando un argomento del costruttore:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

A questo punto l'oggetto non è responsabile della selezione dei `ILogger` da usare. È possibile passare `ILogger` implementazioni senza modificare gli oggetti che dipendono da esso.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Questo modello è denominato [inserimento del costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Un altro modello è l'inserimento di setter, in cui è possibile impostare la dipendenza tramite un metodo setter o una proprietà.

## <a name="simple-dependency-injection-in-signalr"></a>Inserimento di dipendenze semplici in SignalR

Si consideri l'applicazione di chat dell'esercitazione [Introduzione con SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Di seguito è illustrata la classe Hub di tale applicazione:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Si supponga di voler archiviare i messaggi di chat sul server prima di inviarli. È possibile definire un'interfaccia che astrae questa funzionalità e usare DI per inserire l'interfaccia nella classe `ChatHub`.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

L'unico problema è che un'applicazione SignalR non crea direttamente gli hub; SignalR li crea automaticamente. Per impostazione predefinita, SignalR prevede che una classe Hub disponga di un costruttore senza parametri. Tuttavia, è possibile registrare facilmente una funzione per creare istanze dell'hub e usare questa funzione per eseguire un'operazione di. Registrare la funzione chiamando **GlobalHost. DependencyResolver. Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Ora SignalR richiama questa funzione anonima ogni volta che è necessario creare un'istanza di `ChatHub`.

## <a name="ioc-containers"></a>Contenitori IoC

Il codice precedente è adatto per i casi più semplici. Ma è ancora necessario scrivere:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

In un'applicazione complessa con molte dipendenze, potrebbe essere necessario scrivere una grande quantità di codice "cablaggio". Questo codice può essere difficile da gestire, soprattutto se le dipendenze sono annidate. È anche difficile unit test.

Una soluzione consiste nell'usare un contenitore IoC. Un contenitore IoC è un componente software responsabile della gestione delle dipendenze. È possibile registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti. Il contenitore calcola automaticamente le relazioni di dipendenza. Molti contenitori IoC consentono inoltre di controllare elementi come la durata e l'ambito degli oggetti.

> [!NOTE]
> "IoC" sta per "Inversion of Control", che è un modello generale in cui un framework chiama il codice dell'applicazione. Un contenitore IoC crea gli oggetti per l'utente, che "inverte" il normale flusso di controllo.

## <a name="using-ioc-containers-in-signalr"></a>Uso di Contenitori IoC in SignalR

L'applicazione di chat è probabilmente troppo semplice per trarre vantaggio da un contenitore IoC. Si osservi invece l'esempio [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .

L'esempio StockTicker definisce due classi principali:

- `StockTickerHub`: la classe Hub, che gestisce le connessioni client.
- `StockTicker`: un singleton che include i prezzi azionari e li aggiorna periodicamente.

`StockTickerHub` include un riferimento al singleton `StockTicker`, mentre `StockTicker` include un riferimento a **IHubConnectionContext** per la `StockTickerHub`. Usa questa interfaccia per comunicare con le istanze di `StockTickerHub`. Per ulteriori informazioni, vedere la pagina relativa alla [trasmissione server con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).

È possibile usare un contenitore IoC per districare tali dipendenze in un po'. Per prima cosa, è necessario semplificare le classi `StockTickerHub` e `StockTicker`. Nel codice seguente sono stati impostati come commento le parti non necessarie.

Rimuovere il costruttore senza parametri da `StockTickerHub`. Al contrario, si userà sempre DI per creare l'hub.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Per StockTicker, rimuovere l'istanza singleton. In seguito verrà usato il contenitore IoC per controllare la durata del StockTicker. Inoltre, rendere pubblico il costruttore.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Successivamente, è possibile effettuare il refactoring del codice creando un'interfaccia per `StockTicker`. Questa interfaccia verrà usata per separare il `StockTickerHub` dalla classe `StockTicker`.

Visual Studio rende semplice questo tipo di refactoring. Aprire il file StockTicker.cs, fare clic con il pulsante destro del mouse sulla dichiarazione della classe `StockTicker` e selezionare **refactoring** ... **Estrai interfaccia**.

![](dependency-injection/_static/image1.png)

Nella finestra di dialogo **Estrai interfaccia** fare clic su **Seleziona tutto**. Lasciare invariate le altre impostazioni predefinite. Fare clic su **OK**.

![](dependency-injection/_static/image2.png)

Visual Studio crea una nuova interfaccia denominata `IStockTicker`e modifica anche `StockTicker` per derivare da `IStockTicker`.

Aprire il file IStockTicker.cs e modificare l'interfaccia in **public**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

Nella classe `StockTickerHub` modificare le due istanze di `StockTicker` in `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

La creazione di un'interfaccia DI `IStockTicker` non è strettamente necessaria, ma volevo mostrare come può contribuire a ridurre l'accoppiamento tra i componenti nell'applicazione.

## <a name="add-the-ninject-library"></a>Aggiungere la libreria Ninject

Sono disponibili molti contenitori IoC open source per .NET. Per questa esercitazione si userà [Ninject](http://www.ninject.org/). Altre librerie comuni includono [Castle Windsor](http://www.castleproject.org/), [Spring.NET](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)e [StructureMap](http://docs.structuremap.net).

Usare Gestione pacchetti NuGet per installare la [libreria Ninject](https://nuget.org/packages/Ninject/3.0.1.10). In Visual Studio scegliere **Gestione pacchetti NuGet** dal menu **strumenti** > console di **Gestione pacchetti**. Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Sostituire il resolver di dipendenza SignalR

Per usare Ninject all'interno di SignalR, creare una classe che deriva da **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Questa classe esegue l'override dei metodi **GetService** e **GetServices** di **DefaultDependencyResolver**. SignalR chiama questi metodi per creare vari oggetti in fase di esecuzione, incluse le istanze dell'hub, nonché diversi servizi usati internamente da SignalR.

- Il metodo **GetService** crea una singola istanza di un tipo. Eseguire l'override di questo metodo per chiamare il metodo **TryGet** del kernel Ninject. Se il metodo restituisce null, eseguire il fallback al resolver predefinito.
- Il metodo **GetServices** crea una raccolta di oggetti di un tipo specificato. Eseguire l'override di questo metodo per concatenare i risultati di Ninject con i risultati del resolver predefinito.

## <a name="configure-ninject-bindings"></a>Configurare binding Ninject

A questo punto si userà Ninject per dichiarare le associazioni di tipo.

Aprire la classe Startup.cs dell'applicazione (creata manualmente in base alle istruzioni per il pacchetto in `readme.txt`o creata aggiungendo l'autenticazione al progetto). Nel metodo `Startup.Configuration` creare il contenitore Ninject, che Ninject chiama il *kernel*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Creare un'istanza del sistema di risoluzione delle dipendenze personalizzato:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Creare un'associazione per `IStockTicker` come indicato di seguito:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Questo codice dice due cose. Per prima cosa, ogni volta che l'applicazione necessita di un `IStockTicker`, il kernel deve creare un'istanza di `StockTicker`. In secondo luogo, la classe `StockTicker` deve essere creata come oggetto singleton. Ninject creerà un'istanza dell'oggetto e restituirà la stessa istanza per ogni richiesta.

Creare un'associazione per **IHubConnectionContext** come indicato di seguito:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Questo codice crea una funzione anonima che restituisce un **IHubConnection**. Il metodo **WhenInjectedInto** indica a Ninject di utilizzare questa funzione solo quando si creano istanze di `IStockTicker`. Il motivo è che SignalR crea le istanze di **IHubConnectionContext** internamente e non si vuole eseguire l'override del modo in cui SignalR li crea. Questa funzione si applica solo alla classe `StockTicker`.

Passare il resolver di dipendenza al metodo **MapSignalR** aggiungendo una configurazione dell'hub:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Aggiornare il metodo Startup. ConfigureSignalR nella classe startup dell'esempio con il nuovo parametro:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Ora SignalR userà il resolver specificato in **MapSignalR**, anziché il resolver predefinito.

Ecco il listato di codice completo per `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Per eseguire l'applicazione StockTicker in Visual Studio, premere F5. Nella finestra del browser passare a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

L'applicazione ha esattamente le stesse funzionalità di prima. Per una descrizione, vedere [server broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md). Il comportamento non è stato modificato. il codice è stato semplificato per il test, la gestione e l'evoluzione.
