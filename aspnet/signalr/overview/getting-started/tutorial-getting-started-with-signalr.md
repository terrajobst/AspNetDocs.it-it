---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Esercitazione: chat in tempo reale con SignalR 2 | Microsoft Docs'
author: bradygaster
description: In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. È possibile aggiungere SignalR a un'applicazione Web ASP.NET vuota.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600462"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Esercitazione: chat in tempo reale con SignalR 2

Questa esercitazione illustra come usare SignalR per creare un'applicazione di chat in tempo reale. Si aggiunge SignalR a un'applicazione Web ASP.NET vuota e si crea una pagina HTML per inviare e visualizzare i messaggi.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Configurare il progetto
> * Eseguire l'esempio
> * Esaminare il codice

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Prerequisites

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il carico di lavoro di **sviluppo ASP.NET e Web** .

## <a name="set-up-the-project"></a>Configurare il progetto

Questa sezione illustra come usare Visual Studio 2017 e SignalR 2 per creare un'applicazione Web ASP.NET vuota, aggiungere SignalR e creare l'applicazione di chat.

1. In Visual Studio creare un'applicazione Web ASP.NET.

    ![Crea Web](tutorial-getting-started-with-signalr/_static/image2.png)

1. Nella finestra **nuovo progetto ASP.NET-SignalRChat** lasciare **vuoto** selezionato e selezionare **OK**.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.

1. In **Aggiungi nuovo elemento-SignalRChat**selezionare **installato** >  **C# Visual** > **Web** > **SignalR** e quindi selezionare **classe Hub SignalR (v2)** .

1. Assegnare alla classe il nome *ChatHub* e aggiungerlo al progetto.

    Questo passaggio crea il file di classe *ChatHub.cs* e aggiunge un set di file script e riferimenti ad assembly che supportano SignalR al progetto.

1. Sostituire il codice nel nuovo file di classe *ChatHub.cs* con il codice seguente:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.

1. In **Aggiungi nuovo elemento-SignalRChat** selezionare **installato** > **Visual C#**  > **Web** e quindi selezionare **OWIN Startup Class**.

1. Denominare l' *avvio* della classe e aggiungerlo al progetto.

1. Sostituire il codice predefinito nella classe *Startup* con il codice seguente:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **pagina HTML**.

1. Denominare il nuovo *Indice* della pagina e selezionare **OK**.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla pagina HTML creata e selezionare **Imposta come pagina iniziale**.

1. Sostituire il codice predefinito nella pagina HTML con il codice seguente:

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. In **Esplora soluzioni**, espandere **script**.

    Le librerie di script per jQuery e SignalR sono visibili nel progetto.

    > [!IMPORTANT]
    > Gestione pacchetti potrebbe avere installato una versione successiva degli script SignalR.

1. Verificare che i riferimenti allo script nel blocco di codice corrispondano alle versioni dei file di script nel progetto.

    Riferimenti a script dal blocco di codice originale:

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Se non corrispondono, aggiornare il file *HTML* .

1. Dalla barra dei menu selezionare **File** > **Salva tutto**.

## <a name="run-the-sample"></a>Eseguire l'esempio

1. Sulla barra degli strumenti, attivare il **debug degli script** , quindi selezionare il pulsante Riproduci per eseguire l'esempio in modalità di debug.

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image3.png)

1. Quando si apre il browser, immettere un nome per l'identità della chat.

1. Copiare l'URL dal browser, aprire altri due browser e incollare gli URL nelle barre degli indirizzi.

1. In ogni browser immettere un nome univoco.

1. A questo punto, aggiungere un commento e selezionare **Send (Invia**). Ripetere questa ripetizione negli altri browser. I commenti vengono visualizzati in tempo reale.

    > [!NOTE]
    > Questa semplice applicazione di chat non mantiene il contesto di discussione sul server. L'hub trasmette i commenti a tutti gli utenti correnti. Gli utenti che partecipano alla chat in un secondo momento vedranno i messaggi aggiunti a partire dal momento in cui vengono aggiunti.

    Scopri in che modo l'applicazione di chat viene eseguita in tre browser diversi. Quando Tom, Anand e Susan inviano messaggi, tutti i browser vengono aggiornati in tempo reale:

    ![Tutti e tre i browser visualizzano la stessa cronologia delle chat](tutorial-getting-started-with-signalr/_static/image4.png)

1. In **Esplora soluzioni**esaminare il nodo **documenti script** per l'applicazione in esecuzione. È presente un file script denominato *Hub* generato dalla libreria SignalR in fase di esecuzione. Questo file gestisce la comunicazione tra lo script jQuery e il codice lato server.

    ![script Hub generato automaticamente nel nodo documenti script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Esaminare il codice

L'applicazione SignalRChat illustra due attività di sviluppo SignalR di base. Viene illustrato come creare un hub. Il server utilizza tale hub come oggetto di coordinamento principale. L'hub usa la libreria di segnalazione jQuery per inviare e ricevere messaggi.

### <a name="signalr-hubs-in-the-chathubcs"></a>Hub SignalR in ChatHub.cs

Nell'esempio di codice precedente, la classe `ChatHub` deriva dalla classe `Microsoft.AspNet.SignalR.Hub`. La derivazione dalla classe `Hub` è un modo utile per compilare un'applicazione SignalR. È possibile creare metodi pubblici sulla classe Hub e quindi usare tali metodi chiamandoli dagli script in una pagina Web.

Nel codice della chat, i client chiamano il metodo `ChatHub.Send` per inviare un nuovo messaggio. L'Hub invia quindi il messaggio a tutti i client chiamando `Clients.All.broadcastMessage`.

Il metodo `Send` illustra diversi concetti dell'hub:

* Dichiarare i metodi pubblici in un hub in modo che i client possano chiamarli.

* Usare la proprietà dinamica `Microsoft.AspNet.SignalR.Hub.Clients` per comunicare con tutti i client connessi a questo hub.

* Chiamare una funzione sul client (ad esempio la funzione `broadcastMessage`) per aggiornare i client.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>SignalR e jQuery in index. html

La pagina *index. html* nell'esempio di codice illustra come usare la libreria di SignalR jQuery per comunicare con un hub SignalR. Il codice esegue molte attività importanti. Dichiara un proxy per fare riferimento all'hub, dichiara una funzione che il server può chiamare per eseguire il push del contenuto ai client e avvia una connessione per l'invio di messaggi all'hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> In JavaScript il riferimento alla classe server e i relativi membri devono essere camelCase. L'esempio di codice fa C# riferimento alla classe *ChatHub* in JavaScript come `chatHub`.

In questo blocco di codice si crea una funzione di callback nello script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

La classe Hub sul server chiama questa funzione per eseguire il push degli aggiornamenti del contenuto a ogni client. Le due righe che codificano in HTML il contenuto prima di visualizzarlo sono facoltative e mostrano un metodo efficace per impedire l'inserimento di script.

Questo codice apre una connessione con l'hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Questo approccio assicura che il codice stabilisca una connessione prima dell'esecuzione del gestore eventi.

Il codice avvia la connessione e quindi la passa a una funzione per gestire l'evento click sul pulsante **Send** nella pagina HTML.

## <a name="get-the-code"></a>Ottenere il codice

[Scarica progetto completato](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a>Risorse aggiuntive

Per ulteriori informazioni su SignalR, vedere le risorse seguenti:

* [Progetto SignalR](http://signalr.net)

* [Esempi di SignalR e GitHub](https://github.com/SignalR/SignalR)

* [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione vengono illustrate le operazioni seguenti:

> [!div class="checklist"]
> * Configurare il progetto
> * Esecuzione dell'esempio
> * Esame del codice

Passare all'articolo successivo per informazioni su come usare SignalR e MVC 5.
> [!div class="nextstepaction"]
> [SignalR 2 e MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)