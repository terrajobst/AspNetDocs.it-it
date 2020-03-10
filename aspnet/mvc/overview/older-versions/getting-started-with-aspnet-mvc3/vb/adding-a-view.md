---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Aggiunta di una vista (VB) | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: fa200935d83bb26c07b302449a6eba6fd67b5322
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540425"
---
# <a name="adding-a-view-vb"></a>Aggiunta di una visualizzazione (VB)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, è disponibile un progetto Visual Web Developer con codice sorgente VB.NET. [Scaricare la versione di VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce C#, passare alla [ C# versione](../cs/adding-a-view.md) di questa esercitazione.

In questa sezione verrà modificata la classe `HelloWorldController` per utilizzare un file di modello di visualizzazione per incapsulare in modo semplice il processo di generazione di risposte HTML a un client.

Iniziamo usando un modello di vista con il metodo `Index` nella classe `HelloWorldController`. Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Modificare il metodo `Index` per restituire un oggetto `View`, come illustrato di seguito:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

A questo punto, aggiungere un modello di vista al progetto che è possibile richiamare con il metodo `Index`. A tale scopo, fare clic con il pulsante destro del mouse all'interno del metodo `Index` e scegliere **Aggiungi visualizzazione**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** . Lasciare le voci predefinite e fare clic sul pulsante **Aggiungi** .

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

Vengono creati la cartella *MvcMovie\Views\HelloWorld* e il file *MvcMovie\Views\HelloWorld\Index.vbhtml* . È possibile visualizzarli nel **Esplora soluzioni**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Aggiungere codice HTML sotto il tag `<h2>`. Il file *MvcMovie\Views\HelloWorld\Index.vbhtml* modificato è riportato di seguito.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Eseguire l'applicazione e passare al &quot;Hello World&quot; controller (`http://localhost:xxxx/HelloWorld`). Il metodo `Index` nel controller non ha fatto molto lavoro; è stata semplicemente eseguita l'istruzione `return View()`, che indicava che volevamo usare un file modello di visualizzazione per eseguire il rendering di una risposta al client. Poiché non è stato specificato in modo esplicito il nome del file di modello di visualizzazione da usare, per impostazione predefinita, ASP.NET MVC usa il file di visualizzazione *index. vbhtml* nella cartella *\Views\HelloWorld* . L'immagine seguente mostra la stringa hardcoded nella visualizzazione.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Sembra abbastanza valido. Si noti, tuttavia, che la barra del titolo del browser indica &quot;Indice&quot; e il titolo grande nella pagina indica &quot;applicazione MVC.&quot; modifiche.

## <a name="changing-views-and-layout-pages"></a>Modifica delle viste e delle pagine di layout

Per prima cosa, modificare il testo &quot;applicazione MVC.&quot; il testo viene condiviso e visualizzato in ogni pagina. Viene effettivamente visualizzato in una sola posizione del progetto, anche se si trova in ogni pagina dell'applicazione. Passare alla cartella */Views/Shared.* in **Esplora soluzioni** e aprire il file *\_layout. vbhtml* . Questo file è denominato pagina layout ed è la shell &quot;condivisa&quot; utilizzata da tutte le altre pagine.

Si noti la `@RenderBody()` riga di codice accanto alla parte inferiore del file. `RenderBody` è un segnaposto in cui vengono visualizzate tutte le pagine create, &quot;&quot; di cui è stato eseguito il wrapper nella pagina di layout. Modificare l'intestazione di `<h1>` da **&quot;** &quot; dell'applicazione mvc per &quot;&quot;di app del film MVC.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Eseguire l'applicazione e si noti che ora &quot;&quot;app Movie MVC. Fare clic sul collegamento **About (informazioni** ). nella pagina viene visualizzato &quot;&quot;app Movie MVC.

Di seguito è riportato il file completo *\_layout. vbhtml* :

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

A questo punto, modificare il titolo della pagina di indice (visualizzazione).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Aprire *MvcMovie\Views\HelloWorld\Index.vbhtml*. Sono disponibili due posizioni per apportare una modifica: innanzitutto, il testo visualizzato nel titolo del browser e quindi nell'intestazione secondaria (elemento `<h2>`). Le modifiche verranno apportate in modo leggermente diverso, in modo da poter visualizzare il bit di codice modificato che fa parte dell'app.

Eseguire l'applicazione e passare a`http://localhost:xx/HelloWorld`. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. È facile apportare modifiche importanti all'applicazione con piccole modifiche a una vista. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server.

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Il &quot;&quot; di dati, in questo caso il &quot;messaggio Hello World!&quot;, è tuttavia hardcoded. Nell'applicazione MVC sono presenti V (visualizzazioni) e C (controller), ma non M (modello) ancora. A breve verrà illustrato come creare un database e recuperare i dati del modello.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e a parlare dei modelli, tuttavia, parliamo prima di passare le informazioni dal controller a una visualizzazione. Per eseguire il rendering di una risposta HTML a un client, è necessario passare il modello di visualizzazione richiesto. Questi oggetti vengono in genere creati e passati da una classe controller a un modello di visualizzazione e devono contenere solo i dati richiesti dal modello di visualizzazione e non più.

In precedenza con la classe `HelloWorldController`, il metodo di azione `Welcome` ha accettato un `name` e un parametro `numTimes` e quindi ha restituito i valori del parametro al browser. Anziché fare in modo che il controller continui a eseguire direttamente il rendering di questa risposta, verranno inseriti i dati in un contenitore per la visualizzazione. I controller e le visualizzazioni possono usare un oggetto `ViewBag` per conservare i dati. Che verrà passato a un modello di visualizzazione automaticamente e usato per eseguire il rendering della risposta HTML usando il contenuto del contenitore come dati. In questo modo il controller è interessato da una cosa e dal modello di vista con un altro, consentendo di mantenere puliti &quot;la separazione dei problemi&quot; all'interno dell'applicazione.

In alternativa, è possibile definire una classe personalizzata, quindi creare un'istanza di tale oggetto, inserire i dati e passarli alla visualizzazione. Spesso viene chiamato ViewModel, perché si tratta di un modello personalizzato per la visualizzazione. Per piccole quantità di dati, tuttavia, ViewBag funziona benissimo.

Tornare al file *HelloWorldController. vb* modificare il metodo `Welcome` all'interno del controller per inserire il messaggio e NumTimes in ViewBag. ViewBag è un oggetto dinamico. Ciò significa che è possibile inserire il contenuto desiderato. Il ViewBag non dispone di proprietà definite fino a quando non si inserisce un elemento al suo interno.

`HelloWorldController.vb` completa con la nuova classe nello stesso file.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Il ViewBag contiene ora i dati che verranno passati automaticamente alla visualizzazione. Anche in questo caso, è possibile che sia stato passato un oggetto come questo, se è stato apprezzato:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

A questo punto è necessario un modello di `WelcomeView`. Eseguire l'applicazione in modo che il nuovo codice venga compilato. Chiudere il browser, fare clic con il pulsante destro del mouse all'interno del `Welcome` metodo, quindi fare clic su **Aggiungi visualizzazione**.

Ecco come appare la finestra di dialogo **Aggiungi visualizzazione** .

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Aggiungere il codice seguente sotto l'elemento `<h2>` nel nuovo elemento <em>Welcome.</em> file vbhtml. Verrà creato un ciclo e si dirà &quot;Hello&quot; il numero di volte in cui l'utente dice che è necessario.

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Eseguire l'applicazione e passare a `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ora i dati vengono ricavati dall'URL e passati al controller automaticamente. Il controller inserisce i dati in un oggetto `Model` e passa tale oggetto alla visualizzazione. Visualizzazione che Visualizza i dati in formato HTML per l'utente.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Beh, era un tipo di &quot;M&quot; per il modello, ma non per il tipo di database. Creare un database di film con i concetti appresi.

> [!div class="step-by-step"]
> [Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)
