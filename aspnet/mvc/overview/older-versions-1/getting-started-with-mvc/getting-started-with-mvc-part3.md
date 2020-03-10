---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Aggiunta di una vista | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 462b1210c45da67058899193afcea973f3daf122
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581557"
---
# <a name="adding-a-view"></a>Aggiunta di una visualizzazione

di [Scott hanseln](https://github.com/shanselman)

> Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Verrà creata una semplice applicazione Web che legge e scrive da un database. Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.

In questa sezione si esaminerà come è possibile fare in modo che la classe HelloWorldController usi un file modello di visualizzazione per incapsulare in modo semplice la generazione di risposte HTML a un client.

Si inizierà con un modello di visualizzazione con il metodo index. Il metodo è denominato index e si trova in HelloWorldController. Attualmente il metodo index () restituisce una stringa con un messaggio hardcoded nella classe controller.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

A questo punto, modificare il metodo dell'indice in modo che sia simile al seguente:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

A questo punto, aggiungere un modello di vista al progetto che è possibile usare per il metodo index (). A tale scopo, fare clic con il pulsante destro del mouse con il mouse in un punto all'interno del metodo index e scegliere Aggiungi visualizzazione.

![immagine](getting-started-with-mvc-part3/_static/image1.png)

Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione" che fornisce alcune opzioni per la creazione di un modello di visualizzazione che può essere usato dal metodo di indice. Per il momento, non modificare nulla e fare semplicemente clic sul pulsante Aggiungi.

[![finestra di dialogo Aggiungi visualizzazione](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Dopo aver fatto clic su Aggiungi, nella cartella della soluzione verrà visualizzata una nuova cartella e un nuovo file, come illustrato di seguito. A questo punto è disponibile una cartella HelloWorld in views e un file index. aspx all'interno di tale cartella.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Anche il nuovo file di indice è già aperto e pronto per la modifica. Aggiungere testo sotto il primo &lt;H2&gt;index&lt;/H2&gt; like "Hello World".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Eseguire l'applicazione e visitare di nuovo [`http://localhost:xx/HelloWorld`](http://localhostxx) nel browser. Il metodo index nel controller in questo esempio non ha eseguito alcuna operazione, ma ha chiamato "Return View ()" che indicava che volevamo usare un file di modello di visualizzazione per eseguire il rendering di una risposta al client. Poiché non è stato specificato in modo esplicito il nome del file di modello di visualizzazione da usare, ASP.NET MVC ha utilizzato il file di visualizzazione index. aspx nella cartella \Views\HelloWorld. A questo punto viene visualizzata la stringa che è stata hardcoded nella visualizzazione.

[Indice ![-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Sembra abbastanza valido. Si noti, tuttavia, che il titolo del browser dice "index" e il titolo grande nella pagina indica "My MVC Application". Modificare le modifiche.

### <a name="changing-views-and-master-pages"></a>Modifica di viste e pagine master

Prima di tutto, modificare il testo "applicazione MVC". Il testo è condiviso e viene visualizzato in ogni pagina. Viene effettivamente visualizzato in una sola posizione nel codice, anche se si trova in ogni pagina dell'app. Passare alla cartella/Views/Shared. nel Esplora soluzioni e aprire il file site. master. Questo file è denominato pagina master ed è la "Shell" condivisa usata da tutte le altre pagine.

Si noti un testo che indica ContentPlaceholder "MainContent" in questo file.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Il segnaposto è il punto in cui vengono visualizzate tutte le pagine create, "incapsulate" nella pagina master. Provare a modificare il titolo, quindi eseguire l'app e visitare più pagine. Si noterà che l'unica modifica viene visualizzata in più pagine.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

A questo punto, ogni pagina avrà l'intestazione principale, ovvero H1 "My MVC Movie Application". Che gestisce il testo bianco nella parte superiore che viene condiviso tra tutte le pagine.

Ecco il sito. master nel suo complesso con il titolo modificato:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

A questo punto, modificare il titolo della pagina di indice.

Apri/HelloWorld/Index.aspx. Ci sono due posizioni da modificare. In primo luogo, il titolo visualizzato nel titolo del browser, quindi anche l'intestazione secondaria, che è H2. Li rendo leggermente diversi, in modo da poter visualizzare il bit di codice modificato che parte dell'app.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Eseguire l'app e visitare/Movies. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono state modificate. È facile apportare modifiche importanti all'app con piccole modifiche alla visualizzazione.

[Elenco di film ![-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Un po' di "dati", in questo caso "Hello World!" Message) è stato hardcoded. Sono disponibili V (visualizzazioni) e C (controller), ma non M (modello) ancora. A breve verrà illustrato come creare un database e recuperare i dati del modello.

## <a name="passing-a-viewmodel"></a>Passaggio di un ViewModel

Prima di passare a un database e parlare dei modelli, tuttavia, parliamo di "ViewModels". Si tratta di oggetti che rappresentano gli elementi richiesti da un modello di visualizzazione per eseguire il rendering di una risposta HTML a un client. Vengono in genere creati e passati da una classe controller a un modello di visualizzazione e devono contenere solo i dati richiesti dal modello di visualizzazione e non più.

In precedenza, con l'esempio HelloWorld, il metodo di azione Welcome () accettava un nome e un parametro numTimes e lo restituiva al browser. Anziché fare in modo che il controller continui a eseguire direttamente il rendering di questa risposta, si crea invece una piccola classe per contenere tali dati e quindi lo si passa a un modello di visualizzazione per eseguire il rendering della risposta HTML utilizzandola. In questo modo il controller è interessato da una sola cosa e dal modello di vista un altro, consentendo di mantenere pulita la "separazione dei problemi" all'interno dell'applicazione.

Tornare al file HelloWorldController.cs e aggiungere una nuova classe "WelcomeViewModel" e modificare il metodo Welcome all'interno del controller. Di seguito è riportato il HelloWorldController.cs completo con la nuova classe nello stesso file.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Anche se si trova su più righe, il metodo di benvenuto è in realtà solo due istruzioni di codice. La prima istruzione inserisce i due parametri in un oggetto ViewModel e il secondo passa l'oggetto risultante nella visualizzazione.

A questo punto è necessario un modello di visualizzazione iniziale. Fare clic con il pulsante destro del mouse sul metodo Welcome e scegliere Aggiungi visualizzazione. Questa volta, si verificherà "crea una visualizzazione fortemente tipizzata" e si seleziona la classe WelcomeViewModel dall'elenco a discesa. Questa nuova visualizzazione saprà solo WelcomeViewModels e nessun altro tipo di oggetti.

> *Nota: è necessario avere compilato una volta dopo aver aggiunto il WelcomeViewModel per da visualizzare nell'elenco a discesa.*

Ecco come dovrebbe essere visualizzata la finestra di dialogo Aggiungi visualizzazione. Fare clic sul pulsante Aggiungi. ![Aggiungi visualizzazione con cerchio](getting-started-with-mvc-part3/_static/image10.png)

Aggiungere questo codice sotto l'&gt; &lt;H2 nel nuovo Welcome. aspx. Facciamo un ciclo e dico Hello il numero di volte in cui l'utente dice che è necessario.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Si noti anche che, durante la digitazione, è stata rilevata questa visualizzazione del WelcomeViewModel (sono sposati, ricordati?) che si ottengono utili IntelliSense ogni volta che si fa riferimento all'oggetto modello come illustrato nello screenshot seguente:

[Codice sorgente ![NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Eseguire l'applicazione e visitare di nuovo `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`. Ora che i dati sono stati rilevati dall'URL, vengono passati al nostro controller automaticamente, il controller inserisce i dati in un ViewModel e passa tale oggetto alla visualizzazione. Visualizzazione che Visualizza i dati in formato HTML per l'utente.

[![benvenuto-Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Beh, era un tipo di "M" per il modello, ma non il tipo di database. Diamo un'appreso e creiamo un database di filmati.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part2.md)
> [Successivo](getting-started-with-mvc-part4.md)
