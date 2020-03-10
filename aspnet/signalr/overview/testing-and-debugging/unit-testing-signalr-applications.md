---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Unit test di applicazioni SignalR | Microsoft Docs
author: bradygaster
description: Questo articolo descrive come usare le funzionalità di testing unità di SignalR 2,0.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 2cf2e88f141d89971439dc1fc4979849f8dded47
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578673"
---
# <a name="unit-testing-signalr-applications"></a>Unit test di applicazioni SignalR

di [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo articolo descrive l'uso delle funzionalità di testing unità di SignalR 2.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software usate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Unit test delle applicazioni SignalR

È possibile usare le funzionalità di unit test in SignalR 2 per creare unit test per l'applicazione SignalR. SignalR 2 include l'interfaccia [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) , che può essere usata per creare un oggetto fittizio per simulare i metodi dell'hub per il test.

In questa sezione si aggiungeranno unit test per l'applicazione creata nell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md) usando [xUnit.NET](https://github.com/xunit/xunit) e [MOQ](https://github.com/Moq/moq4).

XUnit.net verrà usato per controllare il test; MOQ verrà usato per creare un oggetto [fittizio](http://en.wikipedia.org/wiki/Mock_object) per il test. Se lo si desidera, è possibile usare altri Framework di simulazione. [NSubstitute](http://nsubstitute.github.io/) è anche una scelta ottimale. Questa esercitazione illustra come configurare l'oggetto fittizio in due modi: prima di tutto, usando un oggetto `dynamic` (introdotto in .NET Framework 4) e secondo usando un'interfaccia.

### <a name="contents"></a>Contenuto

Questa esercitazione contiene le sezioni seguenti.

- [Unit test con Dynamic](#dynamic)
- [Unit test per tipo](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Unit test con Dynamic

In questa sezione si aggiungerà un unit test per l'applicazione creata nell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando un oggetto dinamico.

1. Installare l' [estensione xUnit Runner](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) per Visual Studio 2013.
2. Completare l' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md)o scaricare l'applicazione completata da [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Se si usa la versione di download dell'applicazione Introduzione, aprire la **console di gestione pacchetti** e fare clic su **Ripristina** per aggiungere il pacchetto SignalR al progetto.

    ![Ripristinare i pacchetti](unit-testing-signalr-applications/_static/image1.png)
4. Aggiungere un progetto alla soluzione per la unit test. Fare clic con il pulsante destro del mouse sulla soluzione in **Esplora soluzioni** e scegliere **Aggiungi**, **nuovo progetto...** . Nel **C#** nodo selezionare il nodo **Windows** . Selezionare **libreria di classi**. Assegnare al nuovo progetto il nome **TestLibrary** e fare clic su **OK**.

    ![Crea libreria di test](unit-testing-signalr-applications/_static/image2.png)
5. Aggiungere un riferimento nel progetto di libreria di test al progetto SignalRChat. Fare clic con il pulsante destro del mouse sul progetto **TestLibrary** e scegliere **Aggiungi**, **riferimento...** . Selezionare il nodo **progetti** nel nodo **soluzione** e selezionare **SignalRChat**. Fare clic su **OK**.

    ![Aggiungi riferimento al progetto](unit-testing-signalr-applications/_static/image3.png)
6. Aggiungere i pacchetti SignalR, MOQ e XUnit al progetto **TestLibrary** . Nella **console di gestione pacchetti**impostare l'elenco a discesa **progetto predefinito** su **TestLibrary**. Eseguire i comandi seguenti nella finestra della console:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Installare pacchetti](unit-testing-signalr-applications/_static/image4.png)
7. Creare il file di test. Fare clic con il pulsante destro del mouse sul progetto **TestLibrary** e scegliere **Aggiungi...** , **classe**. Assegnare alla nuova classe il nome **tests.cs**.
8. Sostituire il contenuto di Tests.cs con il codice seguente.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    Nel codice precedente, un client di test viene creato usando l'oggetto `Mock` dalla libreria [MOQ](https://github.com/Moq/moq4) , di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2,1, assegnare `dynamic` per il parametro di tipo). L'interfaccia `IHubCallerConnectionContext` è l'oggetto proxy con il quale vengono richiamati i metodi nel client. La funzione `broadcastMessage` viene quindi definita per il client fittizio, in modo che possa essere chiamata dalla classe `ChatHub`. Il motore di test chiama quindi il metodo `Send` della classe `ChatHub`, che a sua volta chiama la funzione `broadcastMessage` fittizia.
9. Compilare la soluzione premendo **F6**.
10. Eseguire lo unit test. In Visual Studio selezionare **test**, **Windows**, **Esplora test**. Nella finestra Esplora test fare clic con il pulsante destro del mouse su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.

    ![Esplora test](unit-testing-signalr-applications/_static/image5.png)
11. Verificare che il test sia stato superato controllando il riquadro inferiore nella finestra Esplora test. La finestra indicherà che il test è stato superato.

    ![Test superato](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Unit test per tipo

In questa sezione si aggiungerà un test per l'applicazione creata nell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md) usando un'interfaccia che contiene il metodo da testare.

1. Completare i passaggi 1-7 nell'esercitazione relativa agli [unit test con Dynamic](#dynamic) .
2. Sostituire il contenuto di Tests.cs con il codice seguente.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    Nel codice precedente viene creata un'interfaccia che definisce la firma del metodo `broadcastMessage` per il quale il motore di test creerà un client fittizio. Viene quindi creato un client fittizio usando l'oggetto `Mock`, di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in signalr 2,1, assegnare `dynamic` per il parametro di tipo). L'interfaccia `IHubCallerConnectionContext` è l'oggetto proxy con il quale vengono richiamati i metodi nel client.

    Il test crea quindi un'istanza di `ChatHub`, quindi crea una versione fittizia del metodo `broadcastMessage`, che a sua volta viene richiamato chiamando il metodo `Send` sull'hub.
3. Compilare la soluzione premendo **F6**.
4. Eseguire lo unit test. In Visual Studio selezionare **test**, **Windows**, **Esplora test**. Nella finestra Esplora test fare clic con il pulsante destro del mouse su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.

    ![Esplora test](unit-testing-signalr-applications/_static/image7.png)
5. Verificare che il test sia stato superato controllando il riquadro inferiore nella finestra Esplora test. La finestra indicherà che il test è stato superato.

    ![Test superato](unit-testing-signalr-applications/_static/image8.png)
