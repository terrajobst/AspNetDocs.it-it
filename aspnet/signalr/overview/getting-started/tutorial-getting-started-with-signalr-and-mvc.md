---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Esercitazione: chat in tempo reale con SignalR 2 e MVC 5 | Microsoft Docs'
author: bradygaster
description: Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. È possibile aggiungere SignalR a un'applicazione MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600478"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Esercitazione: chat in tempo reale con SignalR 2 e MVC 5

Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. Si aggiunge SignalR a un'applicazione MVC 5 e si crea una visualizzazione chat per inviare e visualizzare i messaggi.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Configurare il progetto
> * Eseguire l'esempio
> * Esaminare il codice

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Prerequisites

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il carico di lavoro di **sviluppo ASP.NET e Web** .

## <a name="set-up-the-project"></a>Configurare il progetto

Questa sezione illustra come usare Visual Studio 2017 e SignalR 2 per creare un'applicazione ASP.NET MVC 5 vuota, aggiungere la libreria SignalR e creare l'applicazione di chat.

1. In Visual Studio creare un' C# applicazione ASP.NET destinata a .NET Framework 4,5, denominarla SignalRChat e fare clic su OK.

    ![Crea Web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. In **New ASP.NET Web Application-SignalRMvcChat**selezionare **MVC** , quindi selezionare **Change Authentication**.

1. In **Cambia autenticazione**selezionare **Nessuna autenticazione** e fare clic su **OK**.

    ![Selezionare Nessuna autenticazione](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. In **New ASP.NET Web Application-SignalRMvcChat**fare clic su **OK**.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.

1. In **Aggiungi nuovo elemento-SignalRChat**selezionare **installato** >  **C# Visual** > **Web** > **SignalR** e quindi selezionare **classe Hub SignalR (v2)** .

1. Assegnare alla classe il nome *ChatHub* e aggiungerlo al progetto.

    Questo passaggio crea il file di classe *ChatHub.cs* e aggiunge un set di file script e riferimenti ad assembly che supportano SignalR al progetto.

1. Sostituire il codice nel nuovo file di classe *ChatHub.cs* con il codice seguente:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **classe**.

1. Assegnare un nome all' *avvio* della nuova classe e aggiungerlo al progetto.

1. Sostituire il codice nel file di classe *Startup.cs* con il codice seguente:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. In **Esplora soluzioni**selezionare **controller** > **HomeController.cs**.

1. Aggiungere questo metodo a *HomeController.cs*.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Questo metodo restituisce la visualizzazione **chat** creata in un passaggio successivo.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su **views** > **Home**e scegliere **Aggiungi** >  **visualizzazione**.

1. In **Aggiungi visualizzazione**assegnare un nome alla **chat** nuova visualizzazione e selezionare **Aggiungi**.

1. Sostituire il contenuto di **chat. cshtml** con il codice seguente:

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. In **Esplora soluzioni**, espandere **script**.

    Le librerie di script per jQuery e SignalR sono visibili nel progetto.

    > [!IMPORTANT]
    > Gestione pacchetti potrebbe avere installato una versione successiva degli script SignalR.

1. Verificare che i riferimenti allo script nel blocco di codice corrispondano alle versioni dei file di script nel progetto.

    Riferimenti a script dal blocco di codice originale:

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Se non corrispondono, aggiornare il file con *estensione cshtml* .

1. Dalla barra dei menu selezionare **File** > **Salva tutto**.

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Sulla barra degli strumenti, attivare il **debug degli script** , quindi selezionare il pulsante Riproduci per eseguire l'esempio in modalità di debug.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Quando si apre il browser, immettere un nome per l'identità della chat.

1. Copiare l'URL dal browser, aprire altri due browser e incollare gli URL nelle barre degli indirizzi.

1. In ogni browser immettere un nome univoco.

1. A questo punto, aggiungere un commento e selezionare **Send (Invia**). Ripetere questa ripetizione negli altri browser. I commenti vengono visualizzati in tempo reale.

    > [!NOTE]
    > Questa semplice applicazione di chat non mantiene il contesto di discussione sul server. L'hub trasmette i commenti a tutti gli utenti correnti. Gli utenti che partecipano alla chat in un secondo momento vedranno i messaggi aggiunti a partire dal momento in cui vengono aggiunti.

    Scopri in che modo l'applicazione di chat viene eseguita in tre browser diversi. Quando Tom, Anand e Susan inviano messaggi, tutti i browser vengono aggiornati in tempo reale:

    ![Tutti e tre i browser visualizzano la stessa cronologia delle chat](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. In **Esplora soluzioni**esaminare il nodo **documenti script** per l'applicazione in esecuzione. È presente un file script denominato *Hub* generato dalla libreria SignalR in fase di esecuzione. Questo file gestisce la comunicazione tra lo script jQuery e il codice lato server.

    ![script Hub generato automaticamente nel nodo documenti script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione SignalR chat illustra due attività di sviluppo SignalR di base. Viene illustrato come creare un hub. Il server utilizza tale hub come oggetto di coordinamento principale. L'hub usa la libreria di segnalazione jQuery per inviare e ricevere messaggi.

### <a name="signalr-hubs-in-the-chathubcs"></a>Hub SignalR in ChatHub.cs

Nell'esempio di codice, la classe `ChatHub` deriva dalla classe `Microsoft.AspNet.SignalR.Hub`. La derivazione dalla classe `Hub` è un modo utile per compilare un'applicazione SignalR. È possibile creare metodi pubblici sulla classe Hub e quindi accedere a tali metodi chiamandoli dagli script in una pagina Web.

Nel codice della chat, i client chiamano il metodo `ChatHub.Send` per inviare un nuovo messaggio. L'hub a sua volta invia il messaggio a tutti i client chiamando `Clients.All.addNewMessageToPage`.

Il metodo `Send` illustra diversi concetti dell'hub:

* Dichiarare i metodi pubblici in un hub in modo che i client possano chiamarli.

* Usare la proprietà dinamica `Microsoft.AspNet.SignalR.Hub.Clients` per comunicare con tutti i client connessi a questo hub.

* Chiamare una funzione sul client (ad esempio la funzione `addNewMessageToPage`) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>SignalR e jQuery chat. cshtml

Il file di visualizzazione *chat. cshtml* nell'esempio di codice illustra come usare la libreria jQuery di SignalR per comunicare con un hub SignalR.  Il codice esegue molte attività importanti. Viene creato un riferimento al proxy generato automaticamente per l'hub, viene dichiarata una funzione che il server può chiamare per eseguire il push del contenuto ai client e viene avviata una connessione per l'invio di messaggi all'hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> In JavaScript, il riferimento alla classe server e ai relativi membri si trova in camelCase. L'esempio di codice fa C# riferimento alla classe `ChatHub` in JavaScript come `chatHub`.

In questo blocco di codice si crea una funzione di callback nello script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

La classe Hub sul server chiama questa funzione per eseguire il push degli aggiornamenti del contenuto a ogni client. La chiamata facoltativa alla funzione `htmlEncode` illustra come codificare il contenuto del messaggio in formato HTML prima di visualizzarlo nella pagina. È un modo per impedire l'inserimento di script.

Questo codice apre una connessione con l'hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> Questo approccio assicura che venga stabilita una connessione prima dell'esecuzione del gestore eventi.

Il codice avvia la connessione e quindi la passa a una funzione per gestire l'evento click nel pulsante **Invia** nella pagina della chat.

## <a name="get-the-code"></a>Ottenere il codice

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a>Risorse aggiuntive

Per ulteriori informazioni su SignalR, vedere le risorse seguenti:

* [Progetto SignalR](http://signalr.net)

* [Esempi di SignalR e GitHub](https://github.com/SignalR/SignalR)

* [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Configurare il progetto
> * Esecuzione dell'esempio
> * Esame del codice

Passare all'articolo successivo per informazioni su come creare un'applicazione Web che usa ASP.NET SignalR 2 per fornire funzionalità di messaggistica ad alta frequenza.
> [!div class="nextstepaction"]
> [App Web con messaggistica ad alta frequenza](tutorial-high-frequency-realtime-with-signalr.md)