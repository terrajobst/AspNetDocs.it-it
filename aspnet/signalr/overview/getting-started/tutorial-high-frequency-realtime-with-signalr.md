---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: "Esercitazione: creare un'app in tempo reale ad alta frequenza con SignalR 2 | Microsoft Docs"
author: bradygaster
description: Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558625"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Esercitazione: creare un'app in tempo reale ad alta frequenza con SignalR 2

Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR 2 per fornire funzionalità di messaggistica ad alta frequenza. In questo caso, "messaggistica ad alta frequenza" significa che il server invia aggiornamenti a una frequenza fissa. Inviare un massimo di 10 messaggi al secondo.

L'applicazione creata consente di visualizzare una forma che gli utenti possono trascinare. Il server aggiorna la posizione della forma in tutti i browser connessi in modo che corrisponda alla posizione della forma trascinata usando gli aggiornamenti temporizzati.

I concetti introdotti in questa esercitazione includono applicazioni in giochi in tempo reale e altre applicazioni di simulazione.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Configurare il progetto
> * Creare l'applicazione di base
> * Eseguire il mapping all'hub all'avvio dell'app
> * Aggiungere il client
> * Eseguire l'app
> * Aggiungere il ciclo client
> * Aggiungere il ciclo server
> * Aggiungi animazione liscia

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il carico di lavoro **Sviluppo ASP.NET e Web**.

## <a name="set-up-the-project"></a>Configurare il progetto

In questa sezione viene creato il progetto in Visual Studio 2017.

Questa sezione illustra come usare Visual Studio 2017 per creare un'applicazione Web ASP.NET vuota e aggiungere le librerie SignalR e jQuery. UI.

1. In Visual Studio creare un'applicazione Web ASP.NET.

    ![Crea Web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. Nella finestra **nuova applicazione Web ASP.NET-MoveShapeDemo** lasciare **vuoto** selezionato e selezionare **OK**.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.

1. In **Aggiungi nuovo elemento-MoveShapeDemo**selezionare **installato** >  **C# Visual** > **Web** > **SignalR** e quindi selezionare **classe Hub SignalR (v2)** .

1. Assegnare alla classe il nome *MoveShapeHub* e aggiungerlo al progetto.

    Questo passaggio crea il file della classe *MoveShapeHub.cs* . Simultaneamente, aggiunge un set di file script e riferimenti ad assembly che supportano SignalR al progetto.

1. Fare clic su **Strumenti** > **Gestione Pacchetti NuGet** > **Console di Gestione pacchetti**.

1. Nella **console di gestione pacchetti**eseguire questo comando:

    ```console
    Install-Package jQuery.UI.Combined
    ```

    Il comando installa la libreria dell'interfaccia utente di jQuery. Viene usato per animare la forma.

1. In **Esplora soluzioni**espandere il nodo script.

    ![Riferimenti alla libreria di script](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    Le librerie di script per jQuery, jQueryUI e SignalR sono visibili nel progetto.

## <a name="create-the-base-application"></a>Creare l'applicazione di base

In questa sezione viene creata un'applicazione browser. L'app invia il percorso della forma al server durante ogni evento di spostamento del mouse. Il server trasmette queste informazioni a tutti gli altri client connessi in tempo reale. Per ulteriori informazioni su questa applicazione, vedere le sezioni successive.

1. Aprire il file *MoveShapeHub.cs* .

1. Sostituire il codice nel file *MoveShapeHub.cs* con il codice seguente:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Salvare il file.

La classe `MoveShapeHub` è un'implementazione di un hub SignalR. Come nell'esercitazione [Introduzione con SignalR](tutorial-getting-started-with-signalr.md) , l'Hub dispone di un metodo chiamato direttamente dai client. In questo caso, il client invia un oggetto con le nuove coordinate X e Y della forma al server. Tali coordinate vengono trasmesse a tutti gli altri client connessi. SignalR serializza automaticamente questo oggetto usando JSON.

L'app invia l'oggetto `ShapeModel` al client. Dispone di membri per archiviare la posizione della forma. La versione dell'oggetto nel server dispone anche di un membro per tenere traccia dei dati del client archiviati. Questo oggetto impedisce al server di inviare i dati di un client a se stesso. Questo membro usa l'attributo `JsonIgnore` per impedire alla serializzazione dei dati da parte dell'applicazione e di inviarli di nuovo al client.

## <a name="map-to-the-hub-when-app-starts"></a>Eseguire il mapping all'hub all'avvio dell'app

Si imposta quindi il mapping all'hub all'avvio dell'applicazione. In SignalR 2 l'aggiunta di una classe di avvio OWIN crea il mapping.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **nuovo elemento**.

1. In **Aggiungi nuovo elemento-MoveShapeDemo** selezionare **installato** > **Visual C#**  > **Web** e quindi selezionare **OWIN Startup Class**.

1. Denominare l' *avvio* della classe e selezionare **OK**.

1. Sostituire il codice predefinito nel file *Startup.cs* con il codice seguente:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

La classe di avvio OWIN chiama `MapSignalR` quando l'app esegue il `Configuration` metodo. L'app aggiunge la classe al processo di avvio di OWIN usando l'attributo `OwinStartup` assembly.

## <a name="add-the-client"></a>Aggiungere il client

Aggiungere la pagina HTML per il client.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **pagina HTML**.

1. Assegnare alla pagina il nome **predefinito** e fare clic su **OK**.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *default. html* e scegliere **Imposta come pagina iniziale**.

1. Sostituire il codice predefinito nel file *default. html* con il codice seguente:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. In **Esplora soluzioni**, espandere **script**.

    Le librerie di script per jQuery e SignalR sono visibili nel progetto.

    > [!IMPORTANT]
    > Gestione pacchetti installa una versione successiva degli script SignalR.

1. Aggiornare i riferimenti allo script nel blocco di codice in modo che corrispondano alle versioni dei file di script nel progetto.

Questo codice HTML e JavaScript crea una `div` rossa denominata `shape`. Consente il comportamento di trascinamento della forma usando la libreria jQuery e usa l'evento `drag` per inviare la posizione della forma al server.

## <a name="run-the-app"></a>Eseguire l'app

È possibile eseguire l'app per se'e il funzionamento. Quando si trascina la forma intorno a una finestra del browser, la forma viene spostata anche negli altri browser.

1. Sulla barra degli strumenti, attivare il **debug degli script** , quindi selezionare il pulsante Riproduci per eseguire l'applicazione in modalità di debug.

    ![Screenshot dell'attivazione della modalità di debug da utente e selezione di Play.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    Viene visualizzata una finestra del browser con la forma rossa nell'angolo superiore destro.

1. Copiare l'URL della pagina.

1. Aprire un altro browser e incollare l'URL nella barra degli indirizzi.

1. Trascinare la forma in una delle finestre del browser. Segue la forma nell'altra finestra del browser.

Sebbene l'applicazione funzioni utilizzando questo metodo, non è un modello di programmazione consigliato. Non esiste un limite massimo per il numero di messaggi inviati. Di conseguenza, i client e il server si sovraccaricano di messaggi e prestazioni. Inoltre, l'app Visualizza un'animazione disgiunto nel client. Questa animazione a scatti si verifica perché la forma si sposta immediatamente da ogni metodo. È preferibile che la forma si sposti senza problemi in ogni nuova posizione. Si apprenderà quindi come risolvere questi problemi.

## <a name="add-the-client-loop"></a>Aggiungere il ciclo client

Inviando la posizione della forma a ogni evento di spostamento del mouse, viene creata una quantità di traffico di rete non necessaria. L'app deve limitare i messaggi dal client.

Utilizzare la funzione JavaScript `setInterval` per configurare un ciclo che invia al server le informazioni di nuova posizione a una frequenza fissa. Questo ciclo è una rappresentazione di base di un "ciclo di gioco". Si tratta di una funzione chiamata ripetuta che guida tutte le funzionalità di un gioco.

1. Sostituire il codice client nel file *default. html* con il codice seguente:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > È necessario sostituire di nuovo i riferimenti di script. Devono corrispondere alle versioni degli script nel progetto.

    Questo nuovo codice aggiunge la funzione `updateServerModel`. Viene chiamato su una frequenza fissa. La funzione Invia i dati di posizione al server ogni volta che il flag di `moved` indica che sono presenti nuovi dati di posizione da inviare.

1. Selezionare il pulsante Riproduci per avviare l'applicazione

1. Copiare l'URL della pagina.

1. Aprire un altro browser e incollare l'URL nella barra degli indirizzi.

1. Trascinare la forma in una delle finestre del browser. Segue la forma nell'altra finestra del browser.

Poiché l'app limita il numero di messaggi che vengono inviati al server, l'animazione non verrà visualizzata come smussata prima.

## <a name="add-the-server-loop"></a>Aggiungere il ciclo server

Nell'applicazione corrente, i messaggi inviati dal server al client si escono con la stessa frequenza con cui sono stati ricevuti. Questo traffico di rete presenta un problema simile a quello visualizzato nel client.

L'app può inviare messaggi più spesso del necessario. Di conseguenza, è possibile che la connessione venga allagata. In questa sezione viene descritto come aggiornare il server per aggiungere un timer per limitare la frequenza dei messaggi in uscita.

1. Sostituire il contenuto di `MoveShapeHub.cs` con il codice seguente:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Selezionare il pulsante Riproduci per avviare l'applicazione.

1. Copiare l'URL della pagina.

1. Aprire un altro browser e incollare l'URL nella barra degli indirizzi.

1. Trascinare la forma in una delle finestre del browser.

Questo codice espande il client per aggiungere la classe `Broadcaster`. La nuova classe limita i messaggi in uscita usando la classe `Timer` di .NET Framework.

È opportuno apprendere che l'hub stesso è transitorio. Viene creato ogni volta che è necessario. Quindi, l'app crea il `Broadcaster` come singleton. Usa l'inizializzazione Lazy per rinviare la creazione del `Broadcaster`fino a quando non è necessario. Ciò garantisce che l'app crei completamente la prima istanza dell'hub prima di avviare il timer.

La chiamata alla funzione `UpdateShape` dei client viene quindi spostata al di fuori del metodo `UpdateModel` dell'hub. Non viene più chiamato immediatamente ogni volta che l'app riceve i messaggi in ingresso. Al contrario, l'app invia i messaggi ai client con una frequenza di 25 chiamate al secondo. Il processo viene gestito dal timer `_broadcastLoop` dall'interno della classe `Broadcaster`.

Infine, invece di chiamare direttamente il metodo client dall'hub, la classe `Broadcaster` deve ottenere un riferimento all'hub `_hubContext` operativo corrente. Ottiene il riferimento con l'`GlobalHost`.

## <a name="add-smooth-animation"></a>Aggiungi animazione liscia

L'applicazione è quasi completa, ma è possibile apportare un ulteriore miglioramento. L'app sposta la forma sul client in risposta ai messaggi del server. Anziché impostare la posizione della forma sulla nuova posizione specificata dal server, utilizzare la funzione `animate` della libreria dell'interfaccia utente JQuery. Può spostare la forma senza problemi tra la posizione corrente e quella nuova.

1. Aggiornare il metodo `updateShape` del client nel file *default. html* per avere un aspetto simile al codice evidenziato:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Selezionare il pulsante Riproduci per avviare l'applicazione.

1. Copiare l'URL della pagina.

1. Aprire un altro browser e incollare l'URL nella barra degli indirizzi.

1. Trascinare la forma in una delle finestre del browser.

Lo spostamento della forma nell'altra finestra appare meno Jerky. L'app interpola il proprio movimento nel tempo anziché essere impostato una volta per ogni messaggio in ingresso.

Questo codice sposta la forma dalla posizione precedente a quella nuova. Il server assegna la posizione della forma nel corso dell'intervallo di animazione. In questo caso, si tratta di 100 millisecondi. L'app Cancella qualsiasi animazione precedente in esecuzione sulla forma prima che la nuova animazione venga avviata.

## <a name="get-the-code"></a>Ottenere il codice

[Scarica progetto completato](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a>Risorse aggiuntive

Il paradigma di comunicazione appena acquisita è utile per lo sviluppo di giochi online e altre simulazioni, come [il gioco sparatutto creato con SignalR](https://shootr.azurewebsites.net/).

Per ulteriori informazioni su SignalR, vedere le risorse seguenti:

* [Progetto SignalR](http://signalr.net)

* [Esempi di SignalR e GitHub](https://github.com/SignalR/SignalR)

* [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Configurare il progetto
> * Creazione dell'applicazione di base
> * Mappato all'hub all'avvio dell'app
> * Aggiunta del client
> * Esecuzione dell'app
> * Aggiunta del ciclo client
> * Aggiunta del ciclo server
> * Aggiunta animazione uniforme

Passare all'articolo successivo per informazioni su come creare un'applicazione Web che usa ASP.NET SignalR 2 per fornire funzionalità di broadcast del server.
> [!div class="nextstepaction"]
> [SignalR 2 e broadcast server](tutorial-server-broadcast-with-signalr.md)