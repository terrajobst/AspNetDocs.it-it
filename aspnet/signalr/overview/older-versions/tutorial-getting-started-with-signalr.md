---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Esercitazione: Introduzione con SignalR 1. x | Microsoft Docs'
author: bradygaster
description: Usare ASP.NET SignalR per creare un'applicazione di chat in tempo reale in una pagina HTML.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623543"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a>Esercitazione: Introduzione con SignalR 1. x

di [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. Si aggiungerà SignalR a un'applicazione Web ASP.NET vuota e si creerà una pagina HTML per inviare e visualizzare i messaggi.

## <a name="overview"></a>Panoramica

Questa esercitazione introduce lo sviluppo di SignalR mostrando come creare una semplice applicazione di chat basata su browser. La libreria SignalR viene aggiunta a un'applicazione Web ASP.NET vuota, si crea una classe Hub per l'invio di messaggi ai client e si crea una pagina HTML che consente agli utenti di inviare e ricevere messaggi di chat. Per un'esercitazione simile che illustra come creare un'applicazione di chat in MVC 4 usando una visualizzazione MVC, vedere [Introduzione con SignalR e MVC 4](index.md).

> [!NOTE]
> Questa esercitazione usa la versione Release (1. x) di SignalR. Per informazioni dettagliate sulle modifiche tra SignalR 1. x e 2,0, vedere [aggiornamento di progetti SignalR 1. x](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR è una libreria .NET open source per la creazione di applicazioni Web che richiedono l'interazione dell'utente in tempo reale o aggiornamenti dei dati in tempo reale. Esempi di applicazioni di social networking, giochi multiutente, collaborazione aziendale e notizie, meteo o aggiornamenti finanziari. Queste sono spesso denominate applicazioni in tempo reale.

SignalR semplifica il processo di compilazione di applicazioni in tempo reale. Include una libreria del server ASP.NET e una libreria client JavaScript per semplificare la gestione delle connessioni client-server e il push degli aggiornamenti dei contenuti ai client. È possibile aggiungere la libreria SignalR a un'applicazione ASP.NET esistente per ottenere la funzionalità in tempo reale.

Nell'esercitazione vengono illustrate le seguenti attività di sviluppo SignalR:

- Aggiunta della libreria SignalR a un'applicazione Web ASP.NET.
- Creazione di una classe Hub per eseguire il push del contenuto ai client.
- Uso della libreria jQuery di SignalR in una pagina Web per l'invio di messaggi e la visualizzazione degli aggiornamenti dall'hub.

Lo screenshot seguente illustra l'applicazione di chat in esecuzione in un browser. Ogni nuovo utente può inserire commenti e visualizzare i commenti aggiunti dopo che l'utente ha partecipato alla chat.

![Istanze chat](tutorial-getting-started-with-signalr/_static/image1.png)

Sezioni

- [Configurare il progetto](#setup)
- [Eseguire l'esempio](#run)
- [Esaminare il codice](#code)
- [Passaggi successivi](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurare il progetto

Questa sezione illustra come creare un'applicazione Web ASP.NET vuota, aggiungere SignalR e creare l'applicazione di chat.

Prerequisiti:

- Visual Studio 2010 SP1 o 2012. Se non si dispone di Visual Studio, vedere [download di ASP.NET](https://www.asp.net/downloads) per ottenere lo strumento di sviluppo gratuito visual Studio 2012 Express.
- [Microsoft ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941). Per Visual Studio 2012, questo programma di installazione aggiunge nuove funzionalità di ASP.NET, inclusi i modelli SignalR, a Visual Studio. Per Visual Studio 2010 SP1, un programma di installazione non è disponibile, ma è possibile completare l'esercitazione installando il pacchetto NuGet SignalR come descritto nei passaggi di installazione.

I passaggi seguenti usano Visual Studio 2012 per creare un'applicazione Web ASP.NET vuota e aggiungere la libreria SignalR:

1. In Visual Studio creare un'applicazione Web ASP.NET vuota.

    ![Crea Web vuoto](tutorial-getting-started-with-signalr/_static/image2.png)
2. Aprire la **console di gestione pacchetti** selezionando **strumenti | Gestione pacchetti NuGet | Console di gestione pacchetti**. Immettere il comando seguente nella finestra della console:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Questo comando installa la versione più recente di SignalR 1. x.
3. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi | Classe**. Assegnare alla nuova classe il nome **ChatHub**.
4. In **Esplora soluzioni** espandere il nodo script. Le librerie di script per jQuery e SignalR sono visibili nel progetto.

    ![Riferimenti alla libreria](tutorial-getting-started-with-signalr/_static/image3.png)
5. Sostituire il codice nella classe **ChatHub** con il codice seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi | Nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **classe applicazione globale** e fare clic su **Aggiungi**.

    ![Aggiungi globale](tutorial-getting-started-with-signalr/_static/image4.png)
7. Aggiungere le seguenti istruzioni `using` dopo le istruzioni `using` fornite nella classe Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Aggiungere la seguente riga di codice nel metodo `Application_Start` della classe globale per registrare la route predefinita per gli hub SignalR.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi | Nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare pagina HTML e fare clic su **Aggiungi**.
10. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla pagina HTML appena creata e scegliere **Imposta come pagina iniziale**.
11. Sostituire il codice predefinito nella pagina HTML con il codice seguente.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Salvare tutto** per il progetto.

<a id="run"></a>

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Premere F5 per eseguire il progetto in modalità di debug. La pagina HTML viene caricata in un'istanza del browser e viene richiesto un nome utente.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image5.png)
2. Immettere un nome utente.
3. Copiare l'URL dalla riga di indirizzi del browser e usarlo per aprire altre due istanze del browser. In ogni istanza del browser immettere un nome utente univoco.
4. In ogni istanza del browser aggiungere un commento e fare clic su **Invia**. I commenti devono essere visualizzati in tutte le istanze del browser.

    > [!NOTE]
    > Questa semplice applicazione di chat non mantiene il contesto di discussione sul server. L'hub trasmette i commenti a tutti gli utenti correnti. Gli utenti che partecipano alla chat in un secondo momento vedranno i messaggi aggiunti a partire dal momento in cui vengono aggiunti.

    Lo screenshot seguente illustra l'applicazione di chat in esecuzione in tre istanze del browser, tutte aggiornate quando un'istanza Invia un messaggio:

    ![Browser chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. In **Esplora soluzioni**esaminare il nodo **documenti script** per l'applicazione in esecuzione. È disponibile un file script denominato **Hub** che la libreria SignalR genera dinamicamente in fase di esecuzione. Questo file gestisce la comunicazione tra lo script jQuery e il codice lato server.

    ![Script Hub generato](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione SignalR chat illustra due attività di sviluppo SignalR di base: la creazione di un hub come oggetto di coordinamento principale nel server e l'uso della libreria di segnalazione jQuery per inviare e ricevere messaggi.

### <a name="signalr-hubs"></a>Hub SignalR

Nell'esempio di codice la classe **ChatHub** deriva dalla classe **Microsoft. AspNet. SignalR. Hub** . La derivazione dalla classe **Hub** è un modo utile per creare un'applicazione SignalR. È possibile creare metodi pubblici sulla classe Hub e quindi accedere a tali metodi chiamandoli da script jQuery in una pagina Web.

Nel codice della chat, i client chiamano il metodo **ChatHub. Send** per inviare un nuovo messaggio. L'hub a sua volta invia il messaggio a tutti i client chiamando **clients. all. broadcastMessage**.

Il metodo **Send** illustra diversi concetti sull'hub:

- Dichiarare i metodi pubblici in un hub in modo che i client possano chiamarli.
- Usare la proprietà dinamica **Microsoft. AspNet. SignalR. Hub. clients** per accedere a tutti i client connessi a questo hub.
- Chiamare una funzione jQuery sul client (ad esempio la funzione `broadcastMessage`) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

La pagina HTML nell'esempio di codice illustra come usare la libreria di SignalR jQuery per comunicare con un hub SignalR. Le attività essenziali del codice stanno dichiarando un proxy per fare riferimento all'hub, dichiarando una funzione che il server può chiamare per eseguire il push del contenuto ai client e avviando una connessione per l'invio di messaggi all'hub.

Il codice seguente dichiara un proxy per un hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> In jQuery il riferimento alla classe server e ai relativi membri è in caso di cammello. L'esempio di codice fa C# riferimento alla classe **ChatHub** in jQuery come **ChatHub**.

Il codice seguente illustra come creare una funzione di callback nello script. La classe Hub sul server chiama questa funzione per eseguire il push degli aggiornamenti del contenuto a ogni client. Le due righe che codificano il contenuto HTML prima di visualizzarle sono facoltative e mostrano un modo semplice per impedire l'inserimento di script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Il codice seguente illustra come aprire una connessione con l'hub. Il codice avvia la connessione e quindi la passa a una funzione per gestire l'evento click sul pulsante **Send** nella pagina HTML.

> [!NOTE]
> Questo approccio assicura che la connessione venga stabilita prima dell'esecuzione del gestore eventi.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Passaggi successivi

Si è appreso che SignalR è un Framework per la creazione di applicazioni Web in tempo reale. Sono state inoltre apprese diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe Hub e come inviare e ricevere messaggi dall'hub.

È possibile rendere l'applicazione di esempio in questa esercitazione o in altre applicazioni SignalR disponibili su Internet distribuendo tali applicazioni a un provider di hosting. Microsoft offre hosting web gratuito per un massimo di 10 siti Web in un [account di valutazione gratuito di Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Per una procedura dettagliata su come distribuire l'applicazione SignalR di esempio, vedere [la pagina relativa alla pubblicazione del introduzione di esempio SignalR come sito Web di Microsoft Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Per informazioni dettagliate su come distribuire un progetto Web di Visual Studio in un sito Web di Microsoft Azure, vedere [distribuzione di un'applicazione ASP.NET in un sito Web di Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). Nota: il trasporto WebSocket non è attualmente supportato per siti Web di Microsoft Azure. Quando il trasporto WebSocket non è disponibile, SignalR utilizza gli altri trasporti disponibili come descritto nella sezione trasporti dell' [argomento Introduzione a SignalR](index.md).

Per informazioni sui concetti più avanzati relativi agli sviluppi di SignalR, visitare i siti seguenti per il codice sorgente e le risorse di SignalR:

- [Progetto SignalR](http://signalr.net)
- [Esempi di SignalR e GitHub](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
