---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Esercitazione: Introduzione con SignalR 1. x e MVC 4 | Microsoft Docs'
author: bradygaster
description: Usare ASP.NET SignalR e ASP.NET MVC 4 per creare un'applicazione di chat in tempo reale.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579569"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Esercitazione: Introduzione con SignalR 1. x e MVC 4

di [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questa esercitazione illustra come usare ASP.NET SignalR per creare un'applicazione di chat in tempo reale. Si aggiungerà SignalR a un'applicazione MVC 4 e si creerà una visualizzazione chat per inviare e visualizzare i messaggi.

## <a name="overview"></a>Panoramica

Questa esercitazione illustra lo sviluppo di applicazioni Web in tempo reale con ASP.NET SignalR e ASP.NET MVC 4. L'esercitazione usa lo stesso codice dell'applicazione di chat di [signalr Introduzione esercitazione](tutorial-getting-started-with-signalr.md), ma Mostra come aggiungerlo a un'applicazione MVC 4 basata sul modello Internet.

In questo argomento si apprenderanno le seguenti attività di sviluppo SignalR:

- Aggiunta della libreria SignalR a un'applicazione MVC 4.
- Creazione di una classe Hub per eseguire il push del contenuto ai client.
- Uso della libreria jQuery di SignalR in una pagina Web per l'invio di messaggi e la visualizzazione degli aggiornamenti dall'hub.

Lo screenshot seguente mostra l'applicazione di chat completata in esecuzione in un browser.

![Istanze chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Sezioni

- [Configurare il progetto](#setup)
- [Eseguire l'esempio](#run)
- [Esaminare il codice](#code)
- [Passaggi successivi](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurare il progetto

Prerequisiti:

- Visual Studio 2010 SP1, Visual Studio 2012 o Visual Studio 2012 Express. Se non si dispone di Visual Studio, vedere [download di ASP.NET](https://www.asp.net/downloads) per ottenere lo strumento di sviluppo gratuito visual Studio 2012 Express.
- Per Visual Studio 2010, installare [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

Questa sezione illustra come creare un'applicazione ASP.NET MVC 4, aggiungere la libreria SignalR e creare l'applicazione di chat.

1. 1. In Visual Studio creare un'applicazione ASP.NET MVC 4, denominarla SignalRChat e fare clic su OK.

        > [!NOTE]
        > In VS 2010 selezionare **.NET Framework 4** nel controllo elenco a discesa versione Framework. Il codice SignalR viene eseguito in .NET Framework versioni 4 e 4,5.

        ![Crea Web MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Selezionare il modello applicazione Internet, deselezionare l'opzione per **creare un progetto unit test**e fare clic su OK.

         ![Crea sito Internet MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Aprire **strumenti > gestione pacchetti NuGet > console di gestione pacchetti** ed eseguire il comando seguente. Questo passaggio consente di aggiungere al progetto un set di file script e riferimenti ad assembly che abilitano la funzionalità SignalR.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. In **Esplora soluzioni** espandere la cartella script. Si noti che le librerie di script per SignalR sono state aggiunte al progetto.

         ![Riferimenti alla libreria](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi | Nuova cartella**e aggiungere una nuova cartella denominata **Hub**.
      6. Fare clic con il pulsante destro del mouse sulla cartella **Hub** e scegliere **Aggiungi |** E creare una nuova C# classe denominata **ChatHub.cs**. Questa classe verrà usata come hub server SignalR che invia messaggi a tutti i client.

> [!NOTE]
> Se si usa Visual Studio 2012 e si è installato l' [aggiornamento di ASP.NET and Web Tools 2012,2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), è possibile usare il nuovo modello di elemento SignalR per creare la classe Hub. A tale scopo, fare clic con il pulsante destro del mouse sulla cartella **Hub** e scegliere **Aggiungi | Nuovo elemento**, selezionare la **classe dell'hub SignalR (V1)** e denominare la classe **ChatHub.cs**.

1. Sostituire il codice nella classe **ChatHub** con il codice seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Aprire il file **Global. asax** per il progetto e aggiungere una chiamata al metodo `RouteTable.Routes.MapHubs();` come prima riga di codice nel metodo `Application_Start`. Questo codice registra la route predefinita per gli hub SignalR e deve essere chiamato prima di registrare eventuali altre route. Il metodo `Application_Start` completato è simile all'esempio seguente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Modificare la classe `HomeController` trovata in **Controllers/HomeController. cs** e aggiungere il metodo seguente alla classe. Questo metodo restituisce la visualizzazione **chat** che verrà creata in un passaggio successivo.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Fare clic con il pulsante destro del mouse all'interno del metodo `Chat` appena creato e fare clic su **Aggiungi visualizzazione** per creare un nuovo file di visualizzazione.
5. Nella finestra di dialogo **Aggiungi visualizzazione** verificare che la casella di controllo sia selezionata per l' **utilizzo di un layout o di una pagina master** (deselezionare le altre caselle di controllo), quindi fare clic su **Aggiungi**.

    ![Aggiungere una vista](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Modificare il nuovo file di visualizzazione denominato **chat. cshtml**. Dopo il tag &lt;H2&gt;, incollare la sezione &lt;div&gt; seguente e il blocco di codice `@section scripts` nella pagina. Questo script consente alla pagina di inviare messaggi di chat e visualizzare messaggi dal server. Il codice completo per la visualizzazione chat viene visualizzato nel blocco di codice seguente.

    > [!IMPORTANT]
    > Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, gestione pacchetti potrebbe installare versioni degli script più recenti rispetto alle versioni illustrate in questo argomento. Assicurarsi che i riferimenti agli script nel codice corrispondano alle versioni delle librerie di script installate nel progetto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Salvare tutto** per il progetto.

<a id="run"></a>

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Premere F5 per eseguire il progetto in modalità di debug.
2. Nella riga dell'indirizzo del browser aggiungere **/Home/chat** all'URL della pagina predefinita per il progetto. La pagina di chat viene caricata in un'istanza del browser e viene richiesto un nome utente.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Immettere un nome utente.
4. Copiare l'URL dalla riga di indirizzi del browser e usarlo per aprire altre due istanze del browser. In ogni istanza del browser immettere un nome utente univoco.
5. In ogni istanza del browser aggiungere un commento e fare clic su **Invia**. I commenti devono essere visualizzati in tutte le istanze del browser.

    > [!NOTE]
    > Questa semplice applicazione di chat non mantiene il contesto di discussione sul server. L'hub trasmette i commenti a tutti gli utenti correnti. Gli utenti che partecipano alla chat in un secondo momento vedranno i messaggi aggiunti a partire dal momento in cui vengono aggiunti.
6. Lo screenshot seguente illustra l'applicazione di chat in esecuzione in un browser.

    ![Browser chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. In **Esplora soluzioni**esaminare il nodo **documenti script** per l'applicazione in esecuzione. Questo nodo è visibile in modalità di debug se si utilizza Internet Explorer come browser. È disponibile un file script denominato **Hub** che la libreria SignalR genera dinamicamente in fase di esecuzione. Questo file gestisce la comunicazione tra lo script jQuery e il codice lato server. Se si usa un browser diverso da Internet Explorer, è anche possibile accedere al file di **Hub** dinamici passando direttamente ad esso, ad esempio http://mywebsite/signalr/hubs.

    ![Script Hub generato](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione SignalR chat illustra due attività di sviluppo SignalR di base: la creazione di un hub come oggetto di coordinamento principale nel server e l'uso della libreria di segnalazione jQuery per inviare e ricevere messaggi.

### <a name="signalr-hubs"></a>Hub SignalR

Nell'esempio di codice la classe **ChatHub** deriva dalla classe **Microsoft. AspNet. SignalR. Hub** . La derivazione dalla classe **Hub** è un modo utile per creare un'applicazione SignalR. È possibile creare metodi pubblici sulla classe Hub e quindi accedere a tali metodi chiamandoli da script jQuery in una pagina Web.

Nel codice della chat, i client chiamano il metodo **ChatHub. Send** per inviare un nuovo messaggio. L'hub a sua volta invia il messaggio a tutti i client chiamando **clients. all. addNewMessageToPage**.

Il metodo **Send** illustra diversi concetti sull'hub:

- Dichiarare i metodi pubblici in un hub in modo che i client possano chiamarli.
- Usare la proprietà **Microsoft. AspNet. SignalR. Hub. clients** per accedere a tutti i client connessi a questo hub.
- Chiamare una funzione jQuery sul client (ad esempio la funzione `addNewMessageToPage`) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

Il file di visualizzazione **chat. cshtml** nell'esempio di codice illustra come usare la libreria jQuery di SignalR per comunicare con un hub SignalR. Le attività essenziali del codice creano un riferimento al proxy generato automaticamente per l'hub, dichiarando una funzione che il server può chiamare per eseguire il push del contenuto ai client e avviando una connessione per l'invio di messaggi all'hub.

Il codice seguente dichiara un proxy per un hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> In jQuery il riferimento alla classe server e ai relativi membri è in caso di cammello. L'esempio di codice fa C# riferimento alla classe **ChatHub** in jQuery come **ChatHub**. Se si vuole fare riferimento alla classe `ChatHub` in jQuery con l'intelaiatura Pascal convenzionale come si C#farebbe in, modificare il file della classe ChatHub.cs. Aggiungere un'istruzione `using` per fare riferimento allo spazio dei nomi `Microsoft.AspNet.SignalR.Hubs`. Aggiungere quindi l'attributo `HubName` alla classe `ChatHub`, ad esempio `[HubName("ChatHub")]`. Aggiornare infine il riferimento jQuery alla classe `ChatHub`.

Nel codice seguente viene illustrato come creare una funzione di callback nello script. La classe Hub sul server chiama questa funzione per eseguire il push degli aggiornamenti del contenuto a ogni client. La chiamata facoltativa alla funzione `htmlEncode` illustra come codificare il contenuto del messaggio in formato HTML prima di visualizzarlo nella pagina, in modo da impedire l'inserimento di script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Il codice seguente illustra come aprire una connessione con l'hub. Il codice avvia la connessione e quindi la passa a una funzione per gestire l'evento click nel pulsante **Invia** nella pagina della chat.

> [!NOTE]
> Questo approccio assicura che la connessione venga stabilita prima dell'esecuzione del gestore eventi.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Passaggi successivi

Si è appreso che SignalR è un Framework per la creazione di applicazioni Web in tempo reale. Sono state inoltre apprese diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe Hub e come inviare e ricevere messaggi dall'hub.

Per informazioni sui concetti più avanzati relativi agli sviluppi di SignalR, visitare i siti seguenti per il codice sorgente e le risorse di SignalR:

- [Progetto SignalR](http://signalr.net)
- [Esempi di SignalR e GitHub](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
