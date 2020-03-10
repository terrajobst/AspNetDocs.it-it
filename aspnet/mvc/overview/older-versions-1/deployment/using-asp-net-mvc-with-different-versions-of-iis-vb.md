---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: Uso di ASP.NET MVC con versioni diverse di IIS (VB) | Microsoft Docs
author: microsoft
description: Questa esercitazione illustra come usare ASP.NET MVC e il routing degli URL con diverse versioni di Internet Information Services. Si imparano diverse strategie...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: b754175c853c20eec6be3521376b62d62f33106d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581837"
---
# <a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>Uso di ASP.NET MVC con versioni diverse di IIS (VB)

[Microsoft](https://github.com/microsoft)

> Questa esercitazione illustra come usare ASP.NET MVC e il routing degli URL con diverse versioni di Internet Information Services. Si apprenderanno diverse strategie per l'uso di ASP.NET MVC con IIS 7,0 (modalità classica), IIS 6,0 e le versioni precedenti di IIS.

Il framework MVC ASP.NET dipende dal routing ASP.NET per instradare le richieste del browser alle azioni del controller. Per sfruttare i vantaggi del routing ASP.NET, potrebbe essere necessario eseguire ulteriori passaggi di configurazione sul server Web. Tutto dipende dalla versione di Internet Information Services (IIS) e dalla modalità di elaborazione delle richieste per l'applicazione.

Di seguito è riportato un riepilogo delle diverse versioni di IIS:

- IIS 7,0 (modalità integrata): nessuna configurazione speciale necessaria per usare il routing ASP.NET.
- IIS 7,0 (modalità classica): è necessario eseguire una configurazione speciale per usare il routing ASP.NET.
- IIS 6,0 o versioni precedenti: è necessario eseguire una configurazione speciale per usare il routing ASP.NET.

La versione più recente di IIS è la versione 7,5 (in Win7). IIS 7 di IIS è incluso in Windows Server 2008 e VISTA/SP1 e versioni successive. È anche possibile installare IIS 7,0 in qualsiasi versione del sistema operativo Vista, eccetto Home Basic (vedere [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

IIS 7,0 supporta due modalità per l'elaborazione delle richieste. È possibile usare la modalità integrata o la modalità classica. Non è necessario eseguire passaggi di configurazione speciali quando si usa IIS 7,0 in modalità integrata. Tuttavia, è necessario eseguire una configurazione aggiuntiva quando si usa IIS 7,0 in modalità classica.

Microsoft Windows Server 2003 include IIS 6,0. Non è possibile aggiornare IIS 6,0 a IIS 7,0 quando si usa il sistema operativo Windows Server 2003. Quando si usa IIS 6,0, è necessario eseguire passaggi di configurazione aggiuntivi.

Microsoft Windows XP Professional include IIS 5,1. Quando si usa IIS 5,1, è necessario eseguire passaggi di configurazione aggiuntivi.

Infine, Microsoft Windows 2000 e Microsoft Windows 2000 Professional includono IIS 5,0. Quando si usa IIS 5,0, è necessario eseguire passaggi di configurazione aggiuntivi.

## <a name="integrated-versus-classic-mode"></a>Modalità integrata rispetto alla modalità classica

IIS 7,0 è in grado di elaborare le richieste utilizzando due diverse modalità di elaborazione delle richieste: integrata e classica. La modalità integrata fornisce prestazioni migliori e altre funzionalità. La modalità classica è inclusa per la compatibilità con le versioni precedenti di IIS.

La modalità di elaborazione delle richieste è determinata dal pool di applicazioni. È possibile determinare la modalità di elaborazione utilizzata da un'applicazione Web specifica determinando il pool di applicazioni associato all'applicazione. Attenersi ai passaggi riportati di seguito.

1. Avviare Gestione Internet Information Services
2. Nella finestra connessioni selezionare un'applicazione
3. Nella finestra azioni fare clic sul collegamento **impostazioni di base** per aprire la finestra di dialogo Modifica applicazione (vedere la figura 1).
4. Prendere nota del pool di applicazioni selezionato.

Per impostazione predefinita, IIS è configurato per supportare due pool di applicazioni: **DefaultAppPool** e **AppPool .NET classico**. Se è selezionata l'opzione DefaultAppPool, l'applicazione è in esecuzione in modalità di elaborazione della richiesta integrata. Se è selezionata la versione classica di .NET AppPool, l'applicazione è in esecuzione in modalità di elaborazione delle richieste classiche.

[![finestra di dialogo nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**Figura 1**: rilevamento della modalità di elaborazione delle richieste ([fare clic per visualizzare l'immagine con dimensioni complete](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))

Si noti che è possibile modificare la modalità di elaborazione delle richieste nella finestra di dialogo Modifica applicazione. Fare clic sul pulsante Seleziona e modificare il pool di applicazioni associato all'applicazione. Tenere presente che si verificano problemi di compatibilità quando si modifica un'applicazione ASP.NET dalla modalità classica alla modalità integrata. Per altre informazioni, vedere i seguenti articoli:

- Aggiornamento di ASP.NET 1,1 a IIS 7,0 in Windows Vista e Windows Server 2008-- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- Integrazione di ASP.NET con IIS 7,0- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Se un'applicazione ASP.NET usa il DefaultAppPool, non è necessario eseguire alcuna operazione aggiuntiva per ottenere il routing del ASP.NET (e quindi ASP.NET MVC). Tuttavia, se l'applicazione ASP.NET è configurata in modo da usare il AppPool .NET classico, proseguire con la lettura.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Uso di ASP.NET MVC con le versioni precedenti di IIS

Se è necessario usare ASP.NET MVC con una versione precedente di IIS rispetto a IIS 7,0 o è necessario usare IIS 7,0 in modalità classica, sono disponibili due opzioni. In primo luogo, è possibile modificare la tabella di route per usare le estensioni di file. Ad esempio, anziché richiedere un URL come/Store/Details, è necessario richiedere un URL come/Store.aspx/Details.

La seconda opzione consiste nel creare un elemento denominato *mapping di script con caratteri jolly*. Una mappa di script con caratteri jolly consente di eseguire il mapping di ogni richiesta in ASP.NET Framework.

Se non si ha accesso al server Web (ad esempio, l'applicazione ASP.NET MVC è ospitata da un provider di servizi Internet), è necessario usare la prima opzione. Se non si vuole modificare l'aspetto degli URL e si ha accesso al server Web, è possibile usare la seconda opzione.

Le singole opzioni vengono esaminate in dettaglio nelle sezioni seguenti.

## <a name="adding-extensions-to-the-route-table"></a>Aggiunta di estensioni alla tabella di route

Il modo più semplice per usare il routing ASP.NET con le versioni precedenti di IIS consiste nel modificare la tabella di route nel file Global. asax. Il file Global. asax predefinito e non modificato nel listato 1 configura una route denominata route predefinita.

**Listato 1-Global. asax (non modificato)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

La route predefinita configurata nel listato 1 consente di instradare URL simili ai seguenti:

/Home/Index

/Product/Details/3

/Product

Sfortunatamente, le versioni precedenti di IIS non passano queste richieste a ASP.NET Framework. Pertanto, queste richieste non vengono indirizzate a un controller. Ad esempio, se si effettua una richiesta del browser per l'URL/Home/Index, si otterrà la pagina di errore nella figura 2.

[![finestra di dialogo nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**Figura 2**: ricezione di un errore 404 non trovato ([fare clic per visualizzare l'immagine con dimensioni complete](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))

Le versioni precedenti di IIS mappano solo determinate richieste a ASP.NET Framework. La richiesta deve essere per un URL con l'estensione di file corretta. Ad esempio, viene eseguito il mapping di una richiesta per/SomePage.aspx al framework ASP.NET. Tuttavia, non esiste una richiesta di/SomePage.htm.

Pertanto, per il corretto funzionamento del routing ASP.NET, è necessario modificare la route predefinita in modo da includere un'estensione di file mappata al framework ASP.NET.

Questa operazione viene eseguita utilizzando uno script denominato `registermvc.wsf`. È stata inclusa con la versione ASP.NET MVC 1 in `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, ma a partire da ASP.NET 2 questo script è stato spostato in ASP.NET Futures, disponibile [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).

L'esecuzione di questo script registra una nuova estensione MVC con IIS. Dopo la registrazione dell'estensione MVC, è possibile modificare le route nel file Global. asax in modo che le route usino l'estensione MVC.

Il file Global. asax modificato nel listato 2 funziona con le versioni precedenti di IIS.

**Listato 2-Global. asax (modificato con estensioni)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]

Importante: ricordare di compilare di nuovo l'applicazione ASP.NET MVC dopo aver modificato il file Global. asax.

Nel listato 2 sono presenti due importanti modifiche al file Global. asax. In Global. asax sono ora definite due route. Il modello di URL per la route predefinita, la prima route, ora è simile al seguente:

{controller}. Mvc/{azione}/{ID}

L'aggiunta dell'estensione MVC modifica il tipo di file intercettati dal modulo di routing ASP.NET. Con questa modifica, l'applicazione MVC ASP.NET ora instrada le richieste come le seguenti:

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

La seconda route, ovvero la route radice, è nuova. Questo modello di URL per la route radice è una stringa vuota. Questa route è necessaria per la corrispondenza delle richieste effettuate con la radice dell'applicazione. Ad esempio, la route radice corrisponderà a una richiesta simile alla seguente:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Dopo aver apportato queste modifiche alla tabella di route, è necessario assicurarsi che tutti i collegamenti nell'applicazione siano compatibili con questi nuovi modelli di URL. In altre parole, assicurarsi che tutti i collegamenti includano l'estensione MVC. Se si usa il metodo helper HTML. ActionLink () per generare i collegamenti, non è necessario apportare alcuna modifica.

Invece di usare lo script registermvc. WCF, è possibile aggiungere una nuova estensione a IIS di cui è stato eseguito il mapping a ASP.NET Framework manualmente. Quando si aggiunge manualmente una nuova estensione, assicurarsi che la casella di controllo con etichetta **Verifica che file esistente** non sia selezionata.

## <a name="hosted-server"></a>Server ospitato

Non è sempre possibile accedere al server Web. Se, ad esempio, si ospita l'applicazione ASP.NET MVC utilizzando un provider di hosting Internet, non sarà necessariamente possibile accedere a IIS.

In tal caso, è necessario usare una delle estensioni di file esistenti di cui è stato eseguito il mapping a ASP.NET Framework. Esempi di estensioni di file di cui è stato eseguito il mapping a ASP.NET includono le estensioni aspx, AXD e ashx.

Il file Global. asax modificato nel listato 3, ad esempio, usa l'estensione aspx anziché l'estensione MVC.

**Listato 3-Global. asax (modificato con estensioni. aspx)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

Il file Global. asax nel listato 3 è esattamente uguale al file Global. asax precedente, ad eccezione del fatto che usa l'estensione aspx anziché l'estensione MVC. Non è necessario eseguire alcuna installazione nel server Web remoto per utilizzare l'estensione aspx.

## <a name="creating-a-wildcard-script-map"></a>Creazione di una mappa di script con caratteri jolly

Se non si vuole modificare gli URL per l'applicazione ASP.NET MVC e si ha accesso al server Web, è possibile usare un'opzione aggiuntiva. È possibile creare una mappa di script con caratteri jolly che esegue il mapping di tutte le richieste al server Web a ASP.NET Framework. In questo modo, è possibile usare la tabella di route ASP.NET MVC predefinita con IIS 7,0 (in modalità classica) o IIS 6,0.

Tenere presente che questa opzione fa sì che IIS intercetta ogni richiesta effettuata sul server Web. Sono incluse le richieste per le immagini, le pagine ASP classiche e le pagine HTML. Pertanto, l'abilitazione di un mapping di script con caratteri jolly a ASP.NET ha implicazioni sulle prestazioni.

Di seguito viene illustrato come abilitare un mapping di script con caratteri jolly per IIS 7,0:

1. Selezionare l'applicazione nella finestra connessioni
2. Assicurarsi che sia selezionata la visualizzazione **funzionalità**
3. Fare doppio clic sul pulsante **mapping del gestore**
4. Fare clic sul collegamento **Aggiungi mapping di script con caratteri jolly** (vedere la figura 3).
5. Immettere il percorso del file ASPNET\_ISAPI. dll (è possibile copiare questo percorso dalla mappa di script PageHandlerFactory)
6. Immettere il nome MVC
7. Fare clic sul pulsante **OK**

[![finestra di dialogo nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**Figura 3**: creazione di un mapping di script con caratteri jolly con IIS 7,0 ([fare clic per visualizzare l'immagine con dimensioni complete](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))

Per creare un mapping di script con caratteri jolly con IIS 6,0, attenersi alla procedura seguente:

1. Fare clic con il pulsante destro del mouse sul sito Web e scegliere Proprietà
2. Selezionare la scheda **Home directory**
3. Fare clic sul pulsante di **configurazione**
4. Selezionare la scheda **mapping**
5. Fare clic sul pulsante **Inserisci** (vedere la figura 4).
6. Incollare il percorso del file ASPNET\_ISAPI. dll nel campo eseguibile. è possibile copiare questo percorso dalla mappa di script per i file aspx.
7. Deselezionare la casella **di controllo verifica che il file esista**
8. Fare clic sul pulsante **OK**

[![finestra di dialogo nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**Figura 4**: creazione di un mapping di script con caratteri jolly con IIS 6,0 ([fare clic per visualizzare l'immagine con dimensioni complete](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))

Dopo aver abilitato le mappe di script con caratteri jolly, è necessario modificare la tabella di route nel file Global. asax in modo che includa una route radice. In caso contrario, si otterrà la pagina di errore nella figura 5 quando si effettua una richiesta per la pagina radice dell'applicazione. È possibile utilizzare il file Global. asax modificato nel listato 4.

[![finestra di dialogo nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**Figura 5**: errore della route radice mancante ([fare clic per visualizzare l'immagine con dimensioni complete](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))

**Listato 4-Global. asax (modificato con route radice)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

Dopo aver abilitato una mappa di script con caratteri jolly per IIS 7,0 o IIS 6,0, è possibile effettuare richieste che funzionano con la tabella di route predefinita simile alla seguente:

/

/Home/Index

/Product/Details/3

/Product

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è illustrare come è possibile usare ASP.NET MVC quando si usa una versione precedente di IIS (o IIS 7,0 in modalità classica). Sono stati discussi due metodi per ottenere il routing di ASP.NET per l'utilizzo con le versioni precedenti di IIS: modificare la tabella di route predefinita o creare una mappa di script con caratteri jolly.

La prima opzione richiede la modifica degli URL usati nell'applicazione ASP.NET MVC. Uno dei vantaggi molto significativi di questa prima opzione è che non è necessario accedere a un server Web per modificare la tabella di route. Ciò significa che è possibile usare questa prima opzione anche quando si ospita l'applicazione ASP.NET MVC con una società di hosting Internet.

La seconda opzione consiste nel creare una mappa di script con caratteri jolly. Il vantaggio di questa seconda opzione è che non è necessario modificare gli URL. Lo svantaggio di questa seconda opzione è che può influito sulle prestazioni dell'applicazione MVC ASP.NET.

> [!div class="step-by-step"]
> [Precedente](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
