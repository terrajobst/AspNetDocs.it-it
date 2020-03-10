---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Uso di SignalR con app Web nel servizio app Azure | Microsoft Docs
author: bradygaster
description: Questo documento descrive come configurare un'applicazione SignalR eseguita in Microsoft Azure. Versioni del software usate nell'esercitazione Visual Studio 2013 o Vis...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558702"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a>Uso di SignalR con app Web in Servizio app di Azure

di [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo documento descrive come configurare un'applicazione SignalR eseguita in Microsoft Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) o Visual Studio 2012
> - .NET 4.5
> - SignalR versione 2
> - Azure SDK 2,3 per Visual Studio 2013 o 2012
>
>
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/)o nei [Forum Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).

## <a name="table-of-contents"></a>Sommario

- [Introduzione](#introduction)
- [Distribuzione di un'app Web SignalR nel servizio app Azure](#deploying)
- [Abilitazione di WebSocket nel servizio app Azure](#websocket)
- [Uso del backplane della cache Redis di Azure](#backplane)
- [Passaggi successivi](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Introduzione

ASP.NET SignalR può essere usato per creare un nuovo livello di interattività tra i server e i client Web o .NET. Quando sono ospitati in Azure, le applicazioni SignalR possono sfruttare l'ambiente a disponibilità elevata, scalabile ed efficiente in esecuzione nel cloud.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Distribuzione di un'app Web SignalR nel servizio app Azure

SignalR non aggiunge particolari complicazioni alla distribuzione di un'applicazione in Azure rispetto alla distribuzione in un server locale. Un'applicazione che usa SignalR può essere ospitata in Azure senza apportare modifiche alla configurazione o ad altre impostazioni (anche se per il supporto di WebSocket, vedere [Abilitazione di WebSocket nel servizio app Azure](#websocket) di seguito). Per questa esercitazione, si distribuirà l'applicazione creata nell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md) in Azure.

**Prerequisiti**

- Visual Studio 2013. Se non si dispone di Visual Studio, Visual Studio 2013 Express per il Web è incluso nell'installazione di Azure SDK.
- [Azure sdk 2,3 per Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) o [Azure SDK 2,3 per Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Per completare l'esercitazione, sarà necessaria una sottoscrizione di Azure. È possibile [attivare i benefici per gli abbonati MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)o [iscriversi per ottenere una sottoscrizione di valutazione](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Distribuzione di un'app Web SignalR in Azure

1. Completare l' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md)oppure scaricare il progetto terminato da [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. In Visual Studio selezionare **Compila**, **pubblica la chat di SignalR**.
3. Nella finestra di dialogo "Pubblica Web" selezionare "siti Web di Microsoft Azure".

    ![Selezionare siti Web di Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Se non è stato effettuato l'accesso alla account Microsoft, fare clic su **Accedi...** nella finestra di dialogo "Seleziona sito Web esistente" ed effettuare l'accesso.

    ![Seleziona sito Web esistente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Accedere ad Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. Nella finestra di dialogo "Seleziona sito Web esistente" fare clic su **nuovo**.

    ![Nuovo sito Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. Nella finestra di dialogo "Crea sito in Microsoft Azure" immettere un nome univoco per l'app. Selezionare l'area geografica più vicina nell'elenco a discesa Region (area). Scegliere **Crea**.

    ![Creazione del sito in Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. Nella finestra di dialogo "Pubblica Web" fare clic su **pubblica**.

    ![Pubblica sito](using-signalr-with-azure-web-sites/_static/image6.png)
8. Al termine della pubblicazione dell'app, l'applicazione di chat SignalR ospitata in app Azure app Web del servizio si aprirà in un browser.

    ![Apertura del sito in un browser](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Abilitazione di WebSocket nelle app Web del servizio app Azure

I WebSocket devono essere abilitati in modo esplicito nell'app Web per poter essere usati in un'applicazione SignalR; in caso contrario, verranno usati altri protocolli. per informazioni dettagliate, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports) .

Per usare WebSocket nelle app Web del servizio app Azure, abilitarlo nella sezione di configurazione dell'app Web. A tale scopo, aprire l'app Web nel [portale di gestione di Azure](https://manage.windowsazure.com/)e selezionare Configura.

![Scheda Configura](using-signalr-with-azure-web-sites/_static/image8.png)

Nella parte superiore della pagina di configurazione assicurarsi che .NET 4,5 venga usato per l'app Web.

![Impostazione di .NET Framework versione 4,5](using-signalr-with-azure-web-sites/_static/image9.png)

Nella pagina Configurazione selezionare **on**nell'impostazione **WebSocket** .

![Impostazione WebSocket: on](using-signalr-with-azure-web-sites/_static/image10.png)

Nella parte inferiore della pagina di configurazione selezionare **Salva** per salvare le modifiche.

![Salva impostazioni](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Uso del backplane della cache Redis di Azure

Se si usano più istanze per l'app Web e gli utenti di tali istanze devono interagire tra loro, in modo che, ad esempio, i messaggi di chat creati in un'istanza possano raggiungere gli utenti connessi ad altre istanze, il [backplane della cache Redis di Azure](../performance/scaleout-with-redis.md) deve essere implementato nell'applicazione.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle app Web nel servizio app Azure, vedere [Panoramica delle app Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
