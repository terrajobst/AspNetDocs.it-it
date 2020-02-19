---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Aggiunta di una vista | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 81c2e1f46b08cbc9b5aa5d6c1b36d9d8dc2ba581
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457635"
---
# <a name="adding-a-view"></a>Aggiunta di una visualizzazione

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.

In questa sezione si procederà alla modifica della classe `HelloWorldController` per l'uso dei file di modello di visualizzazione per incapsulare in modo semplice il processo di generazione di risposte HTML a un client.

Verrà creato un file di modello di visualizzazione usando il [motore di visualizzazione Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introdotto con ASP.NET MVC 3. I modelli di visualizzazione basati su Razor hanno un'estensione di file *cshtml* e forniscono un modo elegante per creare l'output C#HTML usando. Razor riduce al minimo il numero di caratteri e le sequenze di tasti richiesti durante la scrittura di un modello di visualizzazione e Abilita un flusso di lavoro di codifica veloce e fluido.

Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Modificare il metodo `Index` per restituire un oggetto `View`, come illustrato nel codice seguente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Il `Index` metodo precedente usa un modello di visualizzazione per generare una risposta HTML al browser. I metodi del controller (noti anche come [metodi di azione](http://rachelappel.com/asp.net-mvc-actionresults-explained)), ad esempio il `Index` metodo precedente, restituiscono in genere un [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una classe derivata da [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), non tipi primitivi come String.

Nel progetto aggiungere un modello di vista che è possibile usare con il metodo `Index`. A tale scopo, fare clic con il pulsante destro del mouse all'interno del metodo `Index` e scegliere **Aggiungi visualizzazione**.

![](adding-a-view/_static/image1.png)

Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** . Lasciare le impostazioni predefinite e fare clic sul pulsante **Aggiungi** :

![](adding-a-view/_static/image2.png)

Vengono creati la cartella *MvcMovie\Views\HelloWorld* e il file *MvcMovie\Views\HelloWorld\Index.cshtml* . È possibile visualizzarli nel **Esplora soluzioni**:

![](adding-a-view/_static/image3.png)

Di seguito è illustrato il file *index. cshtml* creato:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Aggiungere il codice HTML seguente sotto il tag `<h2>`.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Di seguito è riportato il file *MvcMovie\Views\HelloWorld\Index.cshtml* completo.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Se si usa Visual Studio 2012, in Esplora soluzioni fare clic con il pulsante destro del mouse sul file *index. cshtml* e selezionare **Visualizza in controllo pagina**.

![PI](adding-a-view/_static/image5.png)

L' [esercitazione controllo pagina](../../views/using-page-inspector-in-aspnet-mvc.md) contiene altre informazioni su questo nuovo strumento.

In alternativa, eseguire l'applicazione e passare al controller di `HelloWorld` (`http://localhost:xxxx/HelloWorld`). Il metodo `Index` nel controller non ha fatto molto lavoro; è stata eseguita semplicemente l'istruzione `return View()`, che ha specificato che il metodo deve usare un file di modello di visualizzazione per eseguire il rendering di una risposta nel browser. Dal momento che non è stato specificato in modo esplicito il nome del file di modello di visualizzazione da usare, per impostazione predefinita ASP.NET MVC usa il file di visualizzazione *index. cshtml* nella cartella *\Views\HelloWorld* . L'immagine seguente mostra la stringa &quot;Hello dal modello di visualizzazione.&quot; hardcoded nella visualizzazione.

![](adding-a-view/_static/image6.png)

Sembra abbastanza valido. Si noti, tuttavia, che la barra del titolo del browser Mostra &quot;index My ASP.NET an&quot; e il collegamento Big nella parte superiore della pagina indica &quot;il logo qui.&quot; sotto l'&quot;il logo qui.&quot; collegamento sono collegamenti di registrazione e accesso e di seguito sono riportati collegamenti alle pagine Home, about e Contact. Modificare alcune di queste.

## <a name="changing-views-and-layout-pages"></a>Modifica di visualizzazioni e pagine di layout

In primo luogo, si vuole modificare il &quot;logo qui.&quot; titolo nella parte superiore della pagina. Il testo è comune a ogni pagina. Viene effettivamente implementato in una sola posizione del progetto, anche se viene visualizzato in ogni pagina dell'applicazione. Passare alla cartella */Views/Shared.* in **Esplora soluzioni** e aprire il file *\_layout. cshtml* . Questo file è denominato *pagina layout* ed è la shell &quot;condivisa&quot; utilizzata da tutte le altre pagine.

![_LayoutCshtml](adding-a-view/_static/image7.png)

I modelli di layout consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi di applicarlo in più pagine del sito. Trovare la riga `@RenderBody()`. `RenderBody` è un segnaposto dove tutte le pagine specifiche della vista vengono presentate, &quot;incapsulate&quot; nella pagina di layout. Se ad esempio si seleziona il collegamento about, viene eseguito il rendering della visualizzazione *Views\Home\About.cshtml* all'interno del metodo `RenderBody`.

Modificare l'intestazione del titolo del sito nel modello di layout da &quot;logo qui&quot; per &quot;&quot;di film MVC.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Sostituire il contenuto dell'elemento title con il markup seguente:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Eseguire l'applicazione e notare che ora &quot;&quot;film MVC. Fare clic sul collegamento **About (informazioni** ). si vedrà come la pagina mostra anche &quot;Movie MVC&quot;. È stato possibile apportare la modifica una volta nel modello di layout e fare in modo che tutte le pagine del sito riflettano il nuovo titolo.

![](adding-a-view/_static/image8.png)

A questo punto, modificare il titolo della visualizzazione index.

Aprire *MvcMovie\Views\HelloWorld\Index.cshtml*. Sono disponibili due posizioni per apportare una modifica: innanzitutto, il testo visualizzato nel titolo del browser e quindi nell'intestazione secondaria (elemento `<h2>`). Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Per indicare il titolo HTML da visualizzare, il codice precedente imposta una proprietà di `Title` dell'oggetto `ViewBag`, che si trova nel modello di vista *index. cshtml* . Se si esamina il codice sorgente del modello di layout, si noterà che il modello usa questo valore nell'elemento `<title>` come parte della sezione `<head>` del codice HTML modificato in precedenza. Utilizzando questo approccio `ViewBag`, è possibile passare facilmente altri parametri tra il modello di visualizzazione e il file di layout.

Eseguire l'applicazione e passare a `http://localhost:xx/HelloWorld`. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server. Il titolo del browser viene creato con i `ViewBag.Title` impostati nel modello di vista *index. cshtml* e l'app &quot;-Movie aggiuntiva&quot; aggiunta nel file di layout.

Si noti anche che il contenuto del modello di vista *index. cshtml* è stato unito al modello di vista *\_layout. cshtml* ed è stata inviata una singola risposta HTML al browser. I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.

![](adding-a-view/_static/image9.png)

Il &quot;&quot; di dati (in questo caso il &quot;Hello dal modello di visualizzazione&quot; messaggio) è tuttavia hardcoded. L'applicazione MVC ha un &quot;V&quot; (visualizzazione) e si dispone di un &quot;C&quot; (controller), ma non è ancora &quot;M&quot; (modello). A breve verrà illustrato come creare un database e recuperare i dati del modello.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e a parlare dei modelli, tuttavia, parliamo prima di passare le informazioni dal controller a una visualizzazione. Le classi controller vengono richiamate in risposta a una richiesta URL in ingresso. Una classe controller è la posizione in cui si scrive il codice che gestisce le richieste del browser in ingresso, recupera i dati da un database e infine decide il tipo di risposta da restituire al browser. I modelli di visualizzazione possono quindi essere usati da un controller per generare e formattare una risposta HTML al browser.

I controller sono responsabili di fornire tutti i dati o gli oggetti necessari per consentire a un modello di visualizzazione di eseguire il rendering di una risposta al browser. Procedura consigliata: **un modello di vista non deve mai eseguire la logica di business o interagire direttamente con un database**. Al contrario, un modello di visualizzazione dovrebbe funzionare solo con i dati forniti dal controller. Il mantenimento di questo &quot;la separazione dei problemi&quot; consente di mantenere il codice pulito, testabile e più gestibile.

Attualmente, il metodo di azione `Welcome` nella classe `HelloWorldController` accetta un `name` e un parametro `numTimes` e quindi restituisce i valori direttamente al browser. Anziché fare in modo che il controller esegua il rendering di questa risposta come stringa, modificare il controller per usare un modello di visualizzazione. Il modello di vista genererà una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta. È possibile eseguire questa operazione facendo in modo che il controller inserisca i dati dinamici (parametri) necessari per il modello di visualizzazione in un oggetto `ViewBag` a cui il modello di visualizzazione può accedere.

Tornare al file *HelloWorldController.cs* e modificare il metodo `Welcome` per aggiungere un valore `Message` e `NumTimes` all'oggetto `ViewBag`. `ViewBag` è un oggetto dinamico, ovvero è possibile inserire qualsiasi valore desiderato. l'oggetto `ViewBag` non dispone di proprietà definite fino a quando non si inserisce un elemento al suo interno. Il [sistema di associazione di modelli MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) esegue automaticamente il mapping dei parametri denominati (`name` e `numTimes`) dalla stringa di query nella barra degli indirizzi ai parametri nel metodo. Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

A questo punto l'oggetto `ViewBag` contiene i dati che verranno passati automaticamente alla visualizzazione.

Quindi, è necessario un modello di visualizzazione introduttiva. Scegliere **Compila MvcMovie** dal menu **Compila** per verificare che il progetto sia compilato.

Fare quindi clic con il pulsante destro del mouse all'interno del metodo `Welcome` e scegliere **Aggiungi visualizzazione**.

![](adding-a-view/_static/image10.png)

Ecco come appare la finestra di dialogo **Aggiungi visualizzazione** :

![](adding-a-view/_static/image11.png)

Fare clic su **Aggiungi**e quindi aggiungere il codice seguente sotto l'elemento `<h2>` nel nuovo file *Welcome. cshtml* . Verrà creato un ciclo che indica &quot;Hello&quot; il numero di volte indicato dall'utente. Di seguito è riportato il file completo *Welcome. cshtml* .

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Eseguire l'applicazione e passare all'URL seguente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ora i dati vengono ricavati dall'URL e passati al controller usando lo strumento di [associazione di modelli](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Il controller inserisce i dati in un oggetto `ViewBag` e passa tale oggetto alla visualizzazione. La vista Visualizza quindi i dati in formato HTML per l'utente.

![](adding-a-view/_static/image12.png)

Nell'esempio precedente è stato usato un oggetto `ViewBag` per passare i dati dal controller a una visualizzazione. Secondo nell'esercitazione, si userà un modello di visualizzazione per passare i dati da un controller a una vista. L'approccio al modello di visualizzazione per il passaggio dei dati è in genere preferibile rispetto all'approccio del contenitore di viste. Per ulteriori informazioni, vedere il post di Blog [Dynamic V](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con tipizzazione forte.

Beh, era un tipo di &quot;M&quot; per il modello, ma non per il tipo di database. Creare un database di film con i concetti appresi.

> [!div class="step-by-step"]
> [Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)
