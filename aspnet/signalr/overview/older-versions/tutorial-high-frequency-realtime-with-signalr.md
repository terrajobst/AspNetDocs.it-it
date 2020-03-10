---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Tempo reale ad alta frequenza con SignalR 1. x | Microsoft Docs
author: bradygaster
description: Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza. Messaggistica ad alta frequenza in...
ms.author: bradyg
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: e37e63a410952ec170cbbaadeeb54eae7e82b3b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623501"
---
# <a name="high-frequency-realtime-with-signalr-1x"></a>Messaggistica ad alta frequenza in tempo reale con SignalR 1.x

di [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questa esercitazione illustra come creare un'applicazione Web che usa ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza. In questo caso, la messaggistica ad alta frequenza indica gli aggiornamenti inviati a una frequenza fissa. nel caso di questa applicazione, fino a 10 messaggi al secondo.
> 
> Nell'applicazione che verrà creata in questa esercitazione viene visualizzata una forma che gli utenti possono trascinare. La posizione della forma in tutti gli altri browser connessi verrà quindi aggiornata in modo da corrispondere alla posizione della forma trascinata usando gli aggiornamenti temporizzati.
> 
> I concetti introdotti in questa esercitazione includono applicazioni in giochi in tempo reale e altre applicazioni di simulazione.
> 
> I commenti dell'esercitazione sono benvenuti. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com).

## <a name="overview"></a>Panoramica

Questa esercitazione illustra come creare un'applicazione che condivide lo stato di un oggetto con altri browser in tempo reale. L'applicazione che verrà creata è denominata MoveShape. Nella pagina MoveShape viene visualizzato un elemento div HTML che può essere trascinato dall'utente; Quando l'utente trascina il div, la nuova posizione verrà inviata al server, che dirà a tutti gli altri client connessi di aggiornare la posizione della forma in modo che corrisponda.

![Finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

L'applicazione creata in questa esercitazione si basa su una demo di Damian Edwards. Un video contenente questa demo può essere visualizzato [qui](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

L'esercitazione inizierà mostrando come inviare i messaggi di SignalR da ogni evento generato quando la forma viene trascinata. Ogni client connesso aggiornerà quindi la posizione della versione locale della forma ogni volta che viene ricevuto un messaggio.

Mentre l'applicazione funzionerà utilizzando questo metodo, non si tratta di un modello di programmazione consigliato, poiché non esiste un limite massimo per il numero di messaggi inviati, quindi i client e il server potrebbero essere sovraccaricati con i messaggi e le prestazioni verrebbero compromessi . Anche l'animazione visualizzata sul client sarebbe disgiunta, in quanto la forma verrebbe spostata immediatamente da ogni metodo, invece di spostarsi senza intoppi in ogni nuova posizione. Nelle sezioni successive dell'esercitazione verrà illustrato come creare una funzione timer che limita la velocità massima a cui i messaggi vengono inviati dal client o dal server e come spostare la forma in modo uniforme tra le posizioni. La versione finale dell'applicazione creata in questa esercitazione può essere scaricata dalla [raccolta di codici](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Questa esercitazione contiene le sezioni seguenti:

- [Prerequisiti](#prerequisites)
- [Creare il progetto](#createtheproject)
- [Aggiungere i pacchetti NuGet ASP.NET SignalR e JQuery. UI](#nugetpackages)
- [Creare l'applicazione di base](#baseapp)
- [Aggiungere il ciclo client](#clientloop)
- [Aggiungere il ciclo server](#serverloop)
- [Aggiungere Smooth Animation sul client](#animation)
- [Ulteriori passaggi](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

Per questa esercitazione è necessario Visual Studio 2012 o Visual Studio 2010. Se si usa Visual Studio 2010, il progetto utilizzerà .NET Framework 4 anziché .NET Framework 4,5.

Se si usa Visual Studio 2012, è consigliabile installare l' [aggiornamento di ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Questo aggiornamento include nuove funzionalità, ad esempio i miglioramenti alla pubblicazione, alle nuove funzionalità e ai nuovi modelli.

Se si dispone di Visual Studio 2010, verificare che [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) sia installato.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Creare il progetto

In questa sezione verrà creato il progetto in Visual Studio.

1. Scegliere **Nuovo progetto** dal menu **File**.
2. Nella finestra di dialogo **nuovo progetto** espandere **C#** in **modelli** e selezionare **Web**.
3. Selezionare il modello **applicazione Web ASP.NET vuota** , denominare il progetto *MoveShapeDemo*e fare clic su **OK**.

    ![Creazione del nuovo progetto](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Aggiungere i pacchetti NuGet SignalR e JQuery. UI

È possibile aggiungere la funzionalità SignalR a un progetto installando un pacchetto NuGet. Questa esercitazione userà anche il pacchetto JQuery. UI per consentire il trascinamento e l'animazione della forma.

1. Fare clic su **strumenti | Gestione pacchetti NuGet | Console di gestione pacchetti**.
2. Immettere il comando seguente in Gestione pacchetti.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    Il pacchetto SignalR installa una serie di altri pacchetti NuGet come dipendenze. Al termine dell'installazione, sono disponibili tutti i componenti server e client necessari per l'uso di SignalR in un'applicazione ASP.NET.
3. Immettere il comando seguente nella console di gestione pacchetti per installare i pacchetti JQuery e JQuery. UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Creare l'applicazione di base

In questa sezione verrà creata un'applicazione browser che invia il percorso della forma al server durante ogni evento di spostamento del mouse. Il server trasmette quindi queste informazioni a tutti gli altri client connessi così come vengono ricevuti. Questa applicazione verrà espansa nelle sezioni successive.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, **classe**.... Assegnare alla classe il nome **MoveShapeHub** e fare clic su **Aggiungi**.
2. Sostituire il codice nella nuova classe **MoveShapeHub** con il codice seguente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    La classe `MoveShapeHub` precedente è un'implementazione di un hub SignalR. Come nell'esercitazione [Introduzione con SignalR](index.md) , l'Hub dispone di un metodo che i client chiameranno direttamente. In questo caso, il client invierà al server un oggetto contenente le nuove coordinate X e Y della forma, che vengono quindi trasmesse a tutti gli altri client connessi. SignalR serializza automaticamente questo oggetto usando JSON.

    L'oggetto che verrà inviato al client (`ShapeModel`) contiene membri per archiviare la posizione della forma. La versione dell'oggetto nel server contiene anche un membro per tenere traccia dei dati del client archiviati, in modo che a un client specificato non vengano inviati i propri dati. Questo membro usa l'attributo `JsonIgnore` per mantenerne la serializzazione e l'invio al client.
3. Successivamente, si imposterà l'hub all'avvio dell'applicazione. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi | Classe dell'applicazione globale**. Accettare il nome predefinito *globale* e fare clic su **OK**.

    ![Aggiungi classe applicazione globale](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Aggiungere la seguente istruzione `using` dopo le istruzioni **using** fornite nella classe Global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Aggiungere la seguente riga di codice nel metodo `Application_Start` della classe globale per registrare la route predefinita per SignalR.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Il file Global. asax dovrebbe essere simile al seguente:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Successivamente, si aggiungerà il client. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto, quindi scegliere **Aggiungi | Nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **pagina HTML**. Assegnare alla pagina un nome appropriato, ad esempio **default. html**, e fare clic su **Aggiungi**.
7. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla pagina appena creata e scegliere **Imposta come pagina iniziale**.
8. Sostituire il codice predefinito nella pagina HTML con il frammento di codice seguente.

    > [!NOTE]
    > Verificare che i riferimenti agli script riportati di seguito corrispondano ai pacchetti aggiunti al progetto nella cartella Scripts. In Visual Studio 2010, la versione di JQuery e SignalR aggiunta al progetto potrebbe non corrispondere ai numeri di versione seguenti.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Il codice HTML e JavaScript precedente crea una forma div rossa denominata Shape, Abilita il comportamento di trascinamento della forma usando la libreria jQuery e usa l'evento `drag` della forma per inviare la posizione della forma al server.
9. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. la forma nell'altra finestra del browser deve essere spostata.

    ![Finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Aggiungere il ciclo client

Poiché l'invio della posizione della forma in ogni evento di spostamento del mouse creerà una quantità di traffico di rete superflua, è necessario limitare i messaggi del client. Si userà la funzione JavaScript `setInterval` per configurare un ciclo che invia nuove informazioni sulla posizione al server a una frequenza fissa. Questo ciclo è una rappresentazione di base di un "ciclo di gioco", una funzione chiamata ripetuta che guida tutte le funzionalità di un gioco o di un'altra simulazione.

1. Aggiornare il codice client nella pagina HTML in modo che corrisponda al frammento di codice seguente.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    L'aggiornamento precedente aggiunge la funzione `updateServerModel`, che viene chiamata su una frequenza fissa. Questa funzione Invia i dati di posizione al server ogni volta che il flag di `moved` indica che sono presenti nuovi dati di posizione da inviare.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. la forma nell'altra finestra del browser deve essere spostata. Poiché il numero di messaggi inviati al server verrà limitato, l'animazione non verrà visualizzata come smussata nella sezione precedente.

    ![Finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Aggiungere il ciclo server

Nell'applicazione corrente, i messaggi inviati dal server al client si escono con la stessa frequenza con cui vengono ricevuti. Si tratta di un problema simile a quello visualizzato nel client. i messaggi possono essere inviati più spesso di quanto siano necessari e la connessione potrebbe venire allagata di conseguenza. In questa sezione viene descritto come aggiornare il server per implementare un timer per limitare la frequenza dei messaggi in uscita.

1. Sostituire il contenuto di `MoveShapeHub.cs` con il frammento di codice seguente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Il codice precedente espande il client per aggiungere la classe `Broadcaster`, che limita i messaggi in uscita usando la classe `Timer` di .NET Framework.

    Poiché l'hub stesso è transitorio (viene creato ogni volta che è necessario), il `Broadcaster` verrà creato come singleton. L'inizializzazione differita (introdotta in .NET 4) viene usata per rinviarne la creazione fino a quando non è necessaria, assicurandosi che la prima istanza dell'hub venga creata completamente prima dell'avvio del timer.

    La chiamata alla funzione `UpdateShape` dei client viene quindi spostata al di fuori del metodo `UpdateModel` dell'hub, in modo che non venga più chiamata immediatamente ogni volta che vengono ricevuti i messaggi in arrivo. Al contrario, i messaggi ai client verranno inviati a una velocità di 25 chiamate al secondo, gestite dal timer `_broadcastLoop` dall'interno della classe `Broadcaster`.

    Infine, invece di chiamare direttamente il metodo client dall'hub, la classe `Broadcaster` deve ottenere un riferimento all'hub attualmente operativo (`_hubContext`) utilizzando il `GlobalHost`.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. la forma nell'altra finestra del browser deve essere spostata. Non vi sarà alcuna differenza visibile nel browser della sezione precedente, ma il numero di messaggi inviati al client sarà limitato.

    ![Finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Aggiungere Smooth Animation sul client

L'applicazione è quasi completa, ma è possibile apportare un ulteriore miglioramento, nel movimento della forma sul client mentre viene spostata in risposta ai messaggi del server. Anziché impostare la posizione della forma sulla nuova posizione specificata dal server, si userà la funzione `animate` della libreria dell'interfaccia utente JQuery per spostare la forma senza problemi tra la posizione corrente e quella nuova.

1. Aggiornare il metodo `updateShape` del client in modo che somigli al codice evidenziato di seguito:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Il codice precedente sposta la forma dalla posizione precedente a quella nuova fornita dal server nel corso dell'intervallo di animazione (in questo caso, 100 millisecondi). Qualsiasi animazione precedente in esecuzione sulla forma viene cancellata prima dell'avvio della nuova animazione.
2. Avviare l'applicazione premendo F5. Copiare l'URL della pagina e incollarlo in una seconda finestra del browser. Trascinare la forma in una delle finestre del browser. la forma nell'altra finestra del browser deve essere spostata. Lo spostamento della forma nell'altra finestra dovrebbe apparire meno Jerky perché lo spostamento viene interpolato nel tempo anziché essere impostato una volta per ogni messaggio in ingresso.

    ![Finestra dell'applicazione](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Ulteriori passaggi

In questa esercitazione si è appreso come programmare un'applicazione SignalR che invia messaggi ad alta frequenza tra client e server. Questo paradigma di comunicazione è utile per lo sviluppo di giochi online e altre simulazioni, ad esempio [il gioco sparatutto creato con SignalR](http://shootr.signalr.net).

L'applicazione completa creata in questa esercitazione può essere scaricata dalla [raccolta di codici](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Per altre informazioni sui concetti relativi allo sviluppo di SignalR, visitare i siti seguenti per il codice sorgente e le risorse di SignalR:

- [Progetto SignalR](http://signalr.net)
- [Esempi di SignalR e GitHub](https://github.com/SignalR/SignalR)
- [Wiki di SignalR](https://github.com/SignalR/SignalR/wiki)
