---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Esercitazione: Self-host SignalR | Microsoft Docs'
author: bradygaster
description: Questa esercitazione illustra come creare un server SignalR 2 self-hosted e come connettersi a esso con un client JavaScript. Versioni del software usate nell'esercitazione V...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558681"
---
# <a name="tutorial-signalr-self-host"></a>Esercitazione: Self-host SignalR

di [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Scarica progetto completato](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Questa esercitazione illustra come creare un server SignalR 2 self-hosted e come connettersi a esso con un client JavaScript.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Uso di Visual Studio 2012 con questa esercitazione
>
>
> Per usare Visual Studio 2012 con questa esercitazione, seguire questa procedura:
>
> - Aggiornare [Gestione pacchetti](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.
> - Installare l' [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - Nell'installazione guidata piattaforma Web cercare e installare **ASP.NET and Web Tools 2013,1 per Visual Studio 2012**. In questo modo vengono installati i modelli di Visual Studio per le classi SignalR, ad esempio **Hub**.
> - Alcuni modelli, ad esempio **OWIN Startup Class**, non saranno disponibili. per questi, usare invece un file di classe.
>
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

Un server SignalR è in genere ospitato in un'applicazione ASP.NET in IIS, ma può anche essere indipendente (ad esempio in un'applicazione console o in un servizio Windows) usando la libreria Self-host. Questa libreria, come all of SignalR 2, si basa su OWIN ([Open Web Interface for .NET](http://owin.org)). OWIN definisce un'astrazione tra i server Web .NET e le applicazioni Web. OWIN separa l'applicazione Web dal server, rendendo OWIN ideale per l'hosting automatico di un'applicazione Web nel proprio processo, al di fuori di IIS.

I motivi per non ospitare in IIS includono:

- Ambienti in cui IIS non è disponibile o auspicabile, ad esempio un server farm esistente senza IIS.
- È necessario evitare il sovraccarico delle prestazioni di IIS.
- La funzionalità SignalR deve essere aggiunta a un'applicazione esistente eseguita in un servizio Windows, in un ruolo di lavoro di Azure o in un altro processo.

Se una soluzione viene sviluppata come Self-host per motivi di prestazioni, è consigliabile testare anche l'applicazione ospitata in IIS per determinare il vantaggio in termini di prestazioni.

Questa esercitazione contiene le sezioni seguenti:

- [Creazione del server](#server)
- [Accesso al server con un client JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Creazione del server

In questa esercitazione verrà creato un server ospitato in un'applicazione console, ma il server può essere ospitato in qualsiasi tipo di processo, ad esempio un servizio Windows o un ruolo di lavoro di Azure. Per il codice di esempio per l'hosting di un server SignalR in un servizio Windows, vedere la pagina relativa all' [hosting automatico di SignalR in un servizio Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Aprire Visual Studio 2013 con privilegi di amministratore. Selezionare **file**, **nuovo progetto**. Selezionare **Windows** sotto il **nodo C# visivo** nel riquadro **modelli** e selezionare il modello **applicazione console** . Assegnare al nuovo progetto il nome "SignalRSelfHost" e fare clic su **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Aprire la console di gestione pacchetti NuGet selezionando **strumenti** > **gestione pacchetti NuGet** > **console di gestione pacchetti**.
3. Nella console di gestione pacchetti immettere il comando seguente:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Questo comando aggiunge al progetto le librerie a host automatico SignalR 2.
4. Nella console di gestione pacchetti immettere il comando seguente:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Questo comando aggiunge la libreria Microsoft. Owin. CORS al progetto. Questa libreria verrà usata per il supporto tra domini, operazione necessaria per le applicazioni che ospitano SignalR e un client di pagine Web in domini diversi. Poiché si ospiterà il server SignalR e il client Web su porte diverse, ciò significa che tra domini deve essere abilitato per la comunicazione tra questi componenti.
5. Sostituire il contenuto di Program.cs con il codice seguente.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Il codice precedente include tre classi:

    - **Programma**, incluso il metodo **principale** che definisce il percorso principale di esecuzione. In questo metodo, un'applicazione Web di tipo **Startup** viene avviata all'URL specificato (`http://localhost:8080`). Se per l'endpoint è richiesta la sicurezza, è possibile implementare SSL. Per altre informazioni [, vedere Procedura: configurare una porta con un certificato SSL](https://msdn.microsoft.com/library/ms733791.aspx) .
    - **Avvio**, la classe che contiene la configurazione per il server SignalR (l'unica configurazione utilizzata in questa esercitazione è la chiamata a `UseCors`) e la chiamata a `MapSignalR`, che consente di creare route per tutti gli oggetti Hub nel progetto.
    - **MyHub**, la classe dell'hub SignalR che l'applicazione fornirà ai client. Questa classe dispone di un solo metodo, **Send**, che i client chiameranno per trasmettere un messaggio a tutti gli altri client connessi.
6. Compilare l'applicazione ed eseguirla. L'indirizzo che il server sta eseguendo dovrebbe essere visualizzato in una finestra della console.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Se l'esecuzione ha esito negativo con l'eccezione `System.Reflection.TargetInvocationException was unhandled`, sarà necessario riavviare Visual Studio con privilegi di amministratore.
8. Arrestare l'applicazione prima di procedere alla sezione successiva.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Accesso al server con un client JavaScript

In questa sezione si userà lo stesso client JavaScript dell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md). Verrà apportata una sola modifica al client, ovvero definire in modo esplicito l'URL dell'hub. Con un'applicazione indipendente, il server potrebbe non essere necessariamente nello stesso indirizzo dell'URL di connessione (a causa dei proxy inversi e dei bilanciamenti del carico), quindi l'URL deve essere definito in modo esplicito.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Aggiungi**, **nuovo progetto**. Selezionare il nodo **Web** e selezionare il modello **applicazione Web ASP.NET** . Denominare il progetto "JavascriptClient" e fare clic su **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Selezionare il modello **vuoto** e lasciare deselezionate le opzioni rimanenti. Selezionare **Crea progetto**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. Nella console di gestione pacchetti selezionare il progetto "JavascriptClient" nell'elenco a discesa **progetto predefinito** ed eseguire il comando seguente:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Questo comando consente di installare le librerie SignalR e JQuery necessarie nel client.
4. Fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, **nuovo elemento**. Selezionare il nodo **Web** e selezionare pagina HTML. Assegnare alla pagina il nome **default. html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Sostituire il contenuto della nuova pagina HTML con il codice seguente. Verificare che i riferimenti agli script corrispondano agli script nella cartella Scripts del progetto.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Il codice seguente (evidenziato nell'esempio di codice precedente) è l'aggiunta effettuata al client usato nell'esercitazione di acquisizione a stella (oltre all'aggiornamento del codice a SignalR versione 2 beta). Questa riga di codice imposta in modo esplicito l'URL della connessione di base per SignalR nel server.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Fare clic con il pulsante destro del mouse sulla soluzione e selezionare **Imposta progetti di avvio...** . Selezionare il pulsante di opzione **progetti di avvio multipli** e impostare l' **azione** di entrambi i progetti su **Avvia**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Fare clic con il pulsante destro del mouse su "default. html" e selezionare **Imposta come pagina iniziale**.
8. Eseguire l'applicazione. Viene avviato il server e la pagina. Potrebbe essere necessario ricaricare la pagina Web (oppure selezionare **continua** nel debugger) se la pagina viene caricata prima dell'avvio del server.
9. Nel browser fornire un nome utente quando richiesto. Copiare l'URL della pagina in un'altra scheda o finestra del browser e specificare un nome utente diverso. Sarà possibile inviare messaggi da un riquadro del browser all'altro, come nell'esercitazione Introduzione.
