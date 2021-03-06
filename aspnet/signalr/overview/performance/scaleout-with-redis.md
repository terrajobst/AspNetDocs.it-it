---
uid: signalr/overview/performance/scaleout-with-redis
title: Scalabilità orizzontale di SignalR con Redis | Microsoft Docs
author: bradygaster
description: Le versioni del software usate in questo argomento Visual Studio 2013 .NET 4,5 SignalR versione 2 versioni precedenti di questo argomento per informazioni sulle versioni precedenti di...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 58a7affa1769523955adc76455a1c33be6f49751
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579226"
---
# <a name="signalr-scaleout-with-redis"></a>Scale-out di SignalR con Redis

di [Mike Wasson](https://github.com/MikeWasson)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software usate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2,4
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

In questa esercitazione si userà [Redis](http://redis.io/) per distribuire i messaggi attraverso un'applicazione SignalR distribuita in due istanze separate di IIS.

Redis è un archivio chiave-valore in memoria. Supporta inoltre un sistema di messaggistica con un modello di pubblicazione/sottoscrizione. Il backplane di redis di SignalR usa la funzionalità pub/sub per inviare messaggi ad altri server.

![](scaleout-with-redis/_static/image1.png)

Per questa esercitazione, si utilizzeranno tre server:

- Due server che eseguono Windows, che si utilizzeranno per distribuire un'applicazione SignalR.
- Un server che esegue Linux, che sarà usato per eseguire Redis. Per le schermate di questa esercitazione, ho usato Ubuntu 12,04 TLS.

Se non si dispone di tre server fisici da usare, è possibile creare macchine virtuali in Hyper-V. Un'altra opzione consiste nel creare macchine virtuali in Azure.

Anche se in questa esercitazione viene usata l'implementazione di redis ufficiale, è disponibile anche una [porta di Windows Redis di](https://github.com/MSOpenTech/redis) MSOpenTech. L'installazione e la configurazione sono diverse, ma in caso contrario i passaggi sono gli stessi.

> [!NOTE]
>
> La scalabilità orizzontale di SignalR con Redis non supporta i cluster Redis.

## <a name="overview"></a>Panoramica

Prima di arrivare all'esercitazione dettagliata, di seguito viene illustrata una rapida panoramica delle operazioni che si intende eseguire.

1. Installare Redis e avviare il server Redis.
2. Aggiungere i pacchetti NuGet all'applicazione:

    - [Microsoft. AspNet. SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. SignalR. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. Creare un'applicazione SignalR.
4. Per configurare il backplane, aggiungere il codice seguente a Startup.cs:

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu in Hyper-V

Con Windows Hyper-V è possibile creare facilmente una VM Ubuntu in Windows Server.

Scaricare Ubuntu ISO da [http://www.ubuntu.com](http://www.ubuntu.com/).

In Hyper-V aggiungere una nuova macchina virtuale. Nel passaggio **Connetti disco rigido virtuale** selezionare **Crea un disco rigido virtuale**.

![](scaleout-with-redis/_static/image2.png)

Nel passaggio **Opzioni di installazione** selezionare **file di immagine (con estensione ISO)** , fare clic su **Sfoglia**e passare all'installazione ISO di Ubuntu.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Installare Redis

Per scaricare e compilare Redis, seguire la procedura descritta in [http://redis.io/download](http://redis.io/download) .

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Questa operazione compila i file binari di redis nella directory `src`.

Per impostazione predefinita, Redis non richiede una password. Per impostare una password, modificare il file `redis.conf` che si trova nella directory radice del codice sorgente. (Creare una copia di backup del file prima di modificarlo) Aggiungere la direttiva seguente per `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

A questo punto, avviare il server redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Aprire la porta 6379, ovvero la porta predefinita su cui Redis è in ascolto. (È possibile modificare il numero di porta nel file di configurazione).

## <a name="create-the-signalr-application"></a>Creare l'applicazione SignalR

Per creare un'applicazione SignalR, seguire una di queste esercitazioni:

- [Introduzione con SignalR 2,0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introduzione con SignalR 2,0 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Successivamente, l'applicazione di chat verrà modificata per supportare la scalabilità orizzontale con Redis. Per prima cosa, aggiungere il pacchetto NuGet `Microsoft.AspNet.SignalR.StackExchangeRedis` al progetto. In Visual Studio scegliere **Gestione pacchetti NuGet**dal menu **strumenti** e quindi selezionare **console di gestione pacchetti**. Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Aprire quindi il file Startup.cs. Aggiungere il codice seguente al metodo di **configurazione** :

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "Server" è il nome del server che esegue Redis.
- *Port* è il numero di porta
- "password" è la password definita nel file Redis. conf.
- "AppName" è una stringa qualsiasi. SignalR crea un canale di pubblicazione/sottoscrizione Redis con questo nome.

Esempio:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Distribuire ed eseguire l'applicazione

Preparare le istanze di Windows Server per la distribuzione dell'applicazione SignalR.

Aggiungere il ruolo IIS. Includere le funzionalità di "sviluppo di applicazioni", incluso il protocollo WebSocket.

![](scaleout-with-redis/_static/image5.png)

Includere anche il servizio di gestione (elencato in "strumenti di gestione").

![](scaleout-with-redis/_static/image6.png)

**Installare Distribuzione Web 3,0.** Quando si esegue Gestione IIS, viene richiesto di installare la piattaforma Web Microsoft oppure è possibile [scaricare il programma di installazione](https://go.microsoft.com/fwlink/?LinkId=255386). Nel programma di installazione della piattaforma cercare Distribuzione Web e installare Distribuzione Web 3,0

![](scaleout-with-redis/_static/image7.png)

Controllare che il servizio gestione Web sia in esecuzione. In caso contrario, avviare il servizio. Se il servizio gestione Web non è visualizzato nell'elenco dei servizi Windows, assicurarsi di aver installato il servizio di gestione quando è stato aggiunto il ruolo IIS.

Per impostazione predefinita, il servizio gestione Web è in ascolto sulla porta TCP 8172. In Windows Firewall creare una nuova regola in ingresso per consentire il traffico TCP sulla porta 8172. Per ulteriori informazioni, vedere [configurazione delle regole del firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Se si ospitano le macchine virtuali in Azure, è possibile eseguire questa operazione direttamente nel portale di Azure. Vedere [come configurare gli endpoint in una macchina virtuale](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).

A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione e scegliere **pubblica**.

Per informazioni più dettagliate sulla distribuzione Web, vedere [la pagina relativa alla mappa del contenuto della distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Se si distribuisce l'applicazione in due server, è possibile aprire ogni istanza in una finestra del browser separata e verificare che ognuno riceva i messaggi di SignalR dall'altro. Naturalmente, in un ambiente di produzione, i due server si trovano dietro un servizio di bilanciamento del carico.

![](scaleout-with-redis/_static/image8.png)

Se si è curiosi di visualizzare i messaggi inviati a Redis, è possibile usare il client **Redis-CLI** , che viene installato con Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
