---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Aggiunta di una vistaC#() | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: b4a1316feb8d9b7f3ef5ca4755bf1cc5b23e6ef9
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458202"
---
# <a name="adding-a-view-c"></a>Aggiunta di una visualizzazione (C#)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.
> 
> 
> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, C# è disponibile un progetto Visual Web Developer con codice sorgente. [Scaricare la C# versione](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare alla [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.

In questa sezione si procederà alla modifica della classe `HelloWorldController` per l'uso dei file di modello di visualizzazione per incapsulare in modo semplice il processo di generazione di risposte HTML a un client.

Verrà creato un file di modello di visualizzazione usando il nuovo [motore di visualizzazione Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introdotto con ASP.NET MVC 3. I modelli di visualizzazione basati su Razor hanno un'estensione di file *cshtml* e forniscono un modo elegante per creare l'output C#HTML usando. Razor riduce al minimo il numero di caratteri e le sequenze di tasti richiesti durante la scrittura di un modello di visualizzazione e Abilita un flusso di lavoro di codifica veloce e fluido.

Iniziare usando un modello di vista con il metodo `Index` nella classe `HelloWorldController`. Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Modificare il metodo `Index` per restituire un oggetto `View`, come illustrato di seguito:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Questo codice usa un modello di visualizzazione per generare una risposta HTML al browser. Nel progetto aggiungere un modello di vista che è possibile usare con il metodo `Index`. A tale scopo, fare clic con il pulsante destro del mouse all'interno del metodo `Index` e scegliere **Aggiungi visualizzazione**.

![](adding-a-view/_static/image1.png)

Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** . Lasciare le impostazioni predefinite e fare clic sul pulsante **Aggiungi** :

![](adding-a-view/_static/image2.png)

Vengono creati la cartella *MvcMovie\Views\HelloWorld* e il file *MvcMovie\Views\HelloWorld\Index.cshtml* . È possibile visualizzarli nel **Esplora soluzioni**:

![](adding-a-view/_static/image3.png)

Di seguito è illustrato il file *index. cshtml* creato:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Aggiungere codice HTML sotto il tag `<h2>`. Il file *MvcMovie\Views\HelloWorld\Index.cshtml* modificato è riportato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Eseguire l'applicazione e passare al controller di `HelloWorld` (`http://localhost:xxxx/HelloWorld`). Il metodo `Index` nel controller non ha fatto molto lavoro; è stata eseguita semplicemente l'istruzione `return View()`, che ha specificato che il metodo deve usare un file di modello di visualizzazione per eseguire il rendering di una risposta nel browser. Dal momento che non è stato specificato in modo esplicito il nome del file di modello di visualizzazione da usare, per impostazione predefinita ASP.NET MVC usa il file di visualizzazione *index. cshtml* nella cartella *\Views\HelloWorld* . L'immagine seguente mostra la stringa hardcoded nella visualizzazione.

![](adding-a-view/_static/image6.png)

Sembra abbastanza valido. Si noti, tuttavia, che la barra del titolo del browser dice "index" e il titolo grande nella pagina indica "My MVC Application". Modificare le modifiche.

## <a name="changing-views-and-layout-pages"></a>Modifica di visualizzazioni e pagine di layout

In primo luogo, si desidera modificare il titolo "applicazione MVC" nella parte superiore della pagina. Il testo è comune a ogni pagina. In realtà viene implementato in una sola posizione del progetto, anche se viene visualizzato in ogni pagina dell'applicazione. Passare alla cartella */Views/Shared.* in **Esplora soluzioni** e aprire il file *\_layout. cshtml* . Questo file è denominato *pagina layout* ed è la "Shell" condivisa usata da tutte le altre pagine.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

I modelli di layout consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi di applicarlo in più pagine del sito. Si noti la `@RenderBody()` riga nella parte inferiore del file. `RenderBody` è un segnaposto in cui vengono visualizzate tutte le pagine specifiche della visualizzazione create, "incapsulate" nella pagina di layout. Modificare l'intestazione del titolo nel modello di layout da "My MVC Application" a "MVC Movie app".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Eseguire l'applicazione e notare che ora viene visualizzato "MVC Movie app". Fare clic sul collegamento **About (informazioni su** ). verrà visualizzata la pagina "app Movie MVC". È stato possibile apportare la modifica una volta nel modello di layout e fare in modo che tutte le pagine del sito riflettano il nuovo titolo.

![](adding-a-view/_static/image9.png)

Di seguito è riportato il file completo *\_layout. cshtml* :

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

A questo punto, modificare il titolo della pagina di indice (visualizzazione).

Aprire *MvcMovie\Views\HelloWorld\Index.cshtml*. Sono disponibili due posizioni per apportare una modifica: innanzitutto, il testo visualizzato nel titolo del browser e quindi nell'intestazione secondaria (elemento `<h2>`). Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Per indicare il titolo HTML da visualizzare, il codice precedente imposta una proprietà di `Title` dell'oggetto `ViewBag`, che si trova nel modello di vista *index. cshtml* . Se si esamina il codice sorgente del modello di layout, si noterà che il modello usa questo valore nell'elemento `<title>` come parte della sezione `<head>` del codice HTML. Con questo approccio, è possibile passare facilmente altri parametri tra il modello di visualizzazione e il file di layout.

Eseguire l'applicazione e passare a `http://localhost:xx/HelloWorld`. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server.

Si noti anche che il contenuto del modello di vista *index. cshtml* è stato unito al modello di vista *\_layout. cshtml* ed è stata inviata una singola risposta HTML al browser. I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.

![](adding-a-view/_static/image10.png)

Il breve testo di "dati", in questo caso il messaggio "Hello from our View Template!", è tuttavia hardcoded. L'applicazione MVC ha un elemento "V" (vista) ed è stato ottenuto un elemento "C" (controller), ma non ancora un elemento "M" (modello). A breve verrà illustrato come creare un database e recuperare i dati del modello.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e a parlare dei modelli, tuttavia, parliamo prima di passare le informazioni dal controller a una visualizzazione. Le classi controller vengono richiamate in risposta a una richiesta URL in ingresso. Una classe controller è la posizione in cui si scrive il codice che gestisce i parametri in ingresso, recupera i dati da un database e infine decide il tipo di risposta da restituire al browser. I modelli di visualizzazione possono quindi essere usati da un controller per generare e formattare una risposta HTML al browser.

I controller sono responsabili di fornire tutti i dati o gli oggetti necessari per consentire a un modello di visualizzazione di eseguire il rendering di una risposta al browser. Un modello di vista non deve mai eseguire la logica di business o interagire direttamente con un database. Al contrario, dovrebbe funzionare solo con i dati forniti dal controller. Il mantenimento di questa "separazione dei problemi" consente di mantenere il codice pulito e più gestibile.

Attualmente, il metodo di azione `Welcome` nella classe `HelloWorldController` accetta un `name` e un parametro `numTimes` e quindi restituisce i valori direttamente al browser. Anziché fare in modo che il controller esegua il rendering di questa risposta come stringa, modificare il controller per usare un modello di visualizzazione. Il modello di vista genererà una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta. A tale scopo, fare in modo che il controller inserisca i dati dinamici necessari per il modello di visualizzazione in un oggetto `ViewBag` a cui il modello di vista può accedere.

Tornare al file *HelloWorldController.cs* e modificare il metodo `Welcome` per aggiungere un valore `Message` e `NumTimes` all'oggetto `ViewBag`. `ViewBag` è un oggetto dinamico, ovvero è possibile inserire qualsiasi valore desiderato. l'oggetto `ViewBag` non dispone di proprietà definite fino a quando non si inserisce un elemento al suo interno. Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

A questo punto l'oggetto `ViewBag` contiene i dati che verranno passati automaticamente alla visualizzazione.

Quindi, è necessario un modello di visualizzazione introduttiva. Scegliere **Compila MvcMovie** dal menu **debug** per assicurarsi che il progetto sia compilato.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Fare quindi clic con il pulsante destro del mouse all'interno del metodo `Welcome` e scegliere **Aggiungi visualizzazione**. Ecco come appare la finestra di dialogo **Aggiungi visualizzazione** :

![](adding-a-view/_static/image13.png)

Fare clic su **Aggiungi**e quindi aggiungere il codice seguente sotto l'elemento `<h2>` nel nuovo file *Welcome. cshtml* . Verrà creato un ciclo con la dicitura "Hello" per il numero di volte in cui l'utente dice che dovrebbe. Di seguito è riportato il file completo *Welcome. cshtml* .

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Eseguire l'applicazione e passare all'URL seguente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ora i dati vengono ricavati dall'URL e passati al controller automaticamente. Il controller inserisce i dati in un oggetto `ViewBag` e passa tale oggetto alla visualizzazione. La vista Visualizza quindi i dati in formato HTML per l'utente.

![](adding-a-view/_static/image14.png)

Queste operazioni hanno riguardato un tipo di "M" per modello, ma non il tipo di database. Creare un database di film con i concetti appresi.

> [!div class="step-by-step"]
> [Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)
