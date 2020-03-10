---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Scalabilità orizzontale di SignalR con SQL Server | Microsoft Docs
author: bradygaster
description: Le versioni del software usate in questo argomento Visual Studio 2013 .NET 4,5 SignalR versione 2 versioni precedenti di questo argomento per informazioni sulle versioni precedenti di...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 709a9ebf8f3396842bee0d87e621c00ae1418ec1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579184"
---
# <a name="signalr-scaleout-with-sql-server"></a>Scale-out di SignalR con SQL Server

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

In questa esercitazione si userà SQL Server per distribuire i messaggi attraverso un'applicazione SignalR distribuita in due istanze separate di IIS. È anche possibile eseguire questa esercitazione in un singolo computer di test, ma per ottenere l'effetto completo, è necessario distribuire l'applicazione SignalR in due o più server. È inoltre necessario installare SQL Server su uno dei server o su un server dedicato separato. Un'altra opzione consiste nell'eseguire l'esercitazione usando macchine virtuali in Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Prerequisiti

Microsoft SQL Server 2005 o versione successiva. Il backplane supporta le edizioni desktop e server di SQL Server. Non supporta SQL Server Compact Edition o il database SQL di Azure. Se l'applicazione è ospitata in Azure, considerare invece il backplane del bus di servizio.

## <a name="overview"></a>Panoramica

Prima di arrivare all'esercitazione dettagliata, di seguito viene illustrata una rapida panoramica delle operazioni che si intende eseguire.

1. Creare un nuovo database vuoto. Il backplane creerà le tabelle necessarie nel database.
2. Aggiungere i pacchetti NuGet all'applicazione:

    - [Microsoft. AspNet. SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. SignalR. SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Creare un'applicazione SignalR.
4. Per configurare il backplane, aggiungere il codice seguente a Startup.cs:

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   Questo codice configura il backplane con i valori predefiniti per [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Per informazioni sulla modifica di questi valori, vedere [prestazioni di SignalR: metriche di scalabilità orizzontale](signalr-performance.md#scaleout_metrics).

## <a name="configure-the-database"></a>Configurare il database

Decidere se l'applicazione utilizzerà l'autenticazione di Windows o l'autenticazione SQL Server per accedere al database. Indipendentemente da, assicurarsi che l'utente del database disponga delle autorizzazioni per l'accesso, la creazione di schemi e la creazione di tabelle.

Creare un nuovo database per il backplane da usare. È possibile assegnare un nome al database. Non è necessario creare tabelle nel database. il backplane creerà le tabelle necessarie.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Abilita Service Broker

È consigliabile abilitare Service Broker per il database backplane. Service Broker fornisce il supporto nativo per la messaggistica e l'accodamento in SQL Server, che consente al backplane di ricevere gli aggiornamenti in modo più efficiente. Tuttavia, il backplane funziona anche senza Service Broker.

Per verificare se la Service Broker è abilitata, eseguire una query sulla colonna **\_Broker\_abilitato** nella vista del catalogo **sys. databases** .

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Per abilitare Service Broker, usare la query SQL seguente:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Se questa query viene visualizzata come deadlock, assicurarsi che non vi siano applicazioni connesse al database.

Se è stata abilitata la funzionalità di traccia, le tracce indicheranno anche se Service Broker è abilitato.

## <a name="create-a-signalr-application"></a>Creare un'applicazione SignalR

Per creare un'applicazione SignalR, seguire una di queste esercitazioni:

- [Introduzione con SignalR 2,0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introduzione con SignalR 2,0 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Successivamente, l'applicazione di chat verrà modificata per supportare la scalabilità orizzontale con SQL Server. Per prima cosa, aggiungere il pacchetto NuGet SignalR. SqlServer al progetto. In Visual Studio scegliere **Gestione pacchetti NuGet**dal menu **strumenti** e quindi selezionare **console di gestione pacchetti**. Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Aprire quindi il file Startup.cs. Aggiungere il codice seguente al metodo **Configure** :

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Distribuire ed eseguire l'applicazione

Preparare le istanze di Windows Server per la distribuzione dell'applicazione SignalR.

Aggiungere il ruolo IIS. Includere le funzionalità di "sviluppo di applicazioni", incluso il protocollo WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Includere anche il servizio di gestione (elencato in "strumenti di gestione").

![](scaleout-with-sql-server/_static/image5.png)

**Installare Distribuzione Web 3,0.** Quando si esegue Gestione IIS, viene richiesto di installare la piattaforma Web Microsoft oppure è possibile [scaricare il programma di installazione](https://go.microsoft.com/fwlink/?LinkId=255386). Nel programma di installazione della piattaforma cercare Distribuzione Web e installare Distribuzione Web 3,0

![](scaleout-with-sql-server/_static/image6.png)

Controllare che il servizio gestione Web sia in esecuzione. In caso contrario, avviare il servizio. Se il servizio gestione Web non è visualizzato nell'elenco dei servizi Windows, assicurarsi di aver installato il servizio di gestione quando è stato aggiunto il ruolo IIS.

Infine, aprire la porta 8172 per TCP. Si tratta della porta utilizzata dallo strumento Distribuzione Web.

A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione e scegliere **pubblica**.

Per informazioni più dettagliate sulla distribuzione Web, vedere [la pagina relativa alla mappa del contenuto della distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Se si distribuisce l'applicazione in due server, è possibile aprire ogni istanza in una finestra del browser separata e verificare che ognuno riceva i messaggi di SignalR dall'altro. Naturalmente, in un ambiente di produzione, i due server si trovano dietro un servizio di bilanciamento del carico.

![](scaleout-with-sql-server/_static/image7.png)

Dopo aver eseguito l'applicazione, è possibile notare che SignalR ha creato automaticamente le tabelle nel database:

![](scaleout-with-sql-server/_static/image8.png)

SignalR gestisce le tabelle. Fino a quando l'applicazione viene distribuita, non eliminare righe, modificare la tabella e così via.
