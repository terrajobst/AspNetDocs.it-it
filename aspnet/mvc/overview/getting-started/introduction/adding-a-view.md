---
title: Aggiunta di una visualizzazione a un'app MVC
author: Rick-Anderson
description: Aggiunta di una visualizzazione a un'app MVC
ms.author: riande
ms.date: 01/23/2019
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 4b369028aca1e8a6cace60466b8049ccc02a2ec2
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519063"
---
# <a name="adding-a-view"></a>Aggiunta di una visualizzazione

di [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

In questa sezione si procederà alla modifica della classe `HelloWorldController` per l'uso dei file di modello di visualizzazione per incapsulare in modo semplice il processo di generazione di risposte HTML a un client. 

Verrà creato un file di modello di visualizzazione usando il [motore di visualizzazione Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). I modelli di visualizzazione basati su Razor hanno un'estensione di file *cshtml* e forniscono un modo elegante per creare l'output C#HTML usando. Razor riduce al minimo il numero di caratteri e le sequenze di tasti richiesti durante la scrittura di un modello di visualizzazione e Abilita un flusso di lavoro di codifica veloce e fluido.

Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Modificare il metodo `Index` per chiamare il metodo di [visualizzazione](/dotnet/api/microsoft.aspnetcore.mvc.controller.view#Microsoft_AspNetCore_Mvc_Controller_View) controller, come illustrato nel codice seguente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

Il `Index` metodo precedente usa un modello di visualizzazione per generare una risposta HTML al browser. I metodi del controller (noti anche come [metodi di azione](http://rachelappel.com/asp.net-mvc-actionresults-explained)), ad esempio il `Index` metodo precedente, restituiscono in genere un [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una classe derivata da [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), non tipi primitivi come String.

Fare clic con il pulsante destro del mouse sulla cartella *Views\HelloWorld* e scegliere **Aggiungi**, quindi fare clic su **MVC 5 Visualizza pagina con layout (Razor)** .
  
![](adding-a-view/_static/image1.png)   
  
Nella finestra di dialogo **Specifica nome per l'elemento** immettere *index*, quindi fare clic su **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
Nella finestra di dialogo **selezionare una pagina di layout** accettare il valore predefinito **\_layout. cshtml** e fare clic su **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
Nella finestra di dialogo precedente, la cartella *Views\Shared* è selezionata nel riquadro sinistro. Se si dispone di un file di layout personalizzato in un'altra cartella, è possibile selezionarlo. Il file di layout verrà discusso più avanti nell'esercitazione

Il file *MvcMovie\Views\HelloWorld\Index.cshtml* viene creato.

![](adding-a-view/_static/image4.png)

Aggiungere il markup evidenziato seguente.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Fare clic con il pulsante destro del mouse sul file *index. cshtml* e selezionare **Visualizza nel browser**.

![PI](adding-a-view/_static/image5.png)

È anche possibile fare clic con il pulsante destro del mouse sul file *index. cshtml* e selezionare **Visualizza in controllo pagina.** Per ulteriori informazioni, vedere l' [esercitazione controllo pagina](../../views/using-page-inspector-in-aspnet-mvc.md) .

In alternativa, eseguire l'applicazione e passare al controller di `HelloWorld` (`http://localhost:xxxx/HelloWorld`). Il metodo `Index` nel controller non ha fatto molto lavoro; è stata eseguita semplicemente l'istruzione `return View()`, che ha specificato che il metodo deve usare un file di modello di visualizzazione per eseguire il rendering di una risposta nel browser. Dal momento che non è stato specificato in modo esplicito il nome del file di modello di visualizzazione da usare, per impostazione predefinita ASP.NET MVC usa il file di visualizzazione *index. cshtml* nella cartella *\Views\HelloWorld* . L'immagine seguente mostra la stringa &quot;Hello dal modello di visualizzazione.&quot; hardcoded nella visualizzazione.

![](adding-a-view/_static/image6.png)

Sembra abbastanza valido. Si noti, tuttavia, che la barra del titolo del browser Mostra "index-My ASP.NET Application" e il collegamento Big nella parte superiore della pagina indica "Application Name". A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sulle tre barre in alto a destra per visualizzare i collegamenti **Home**, **informazioni**, **contatti**, **registrazione** e **accesso** .

## <a name="changing-views-and-layout-pages"></a>Modifica di visualizzazioni e pagine di layout

In primo luogo, si desidera modificare il nome dell'applicazione &quot;&quot; collegamento nella parte superiore della pagina. Il testo è comune a ogni pagina. Viene effettivamente implementato in una sola posizione del progetto, anche se viene visualizzato in ogni pagina dell'applicazione. Passare alla cartella */Views/Shared.* in **Esplora soluzioni** e aprire il file *\_layout. cshtml* . Questo file è denominato *pagina layout* e si trova nella cartella condivisa utilizzata da tutte le altre pagine.

![_LayoutCshtml](adding-a-view/_static/image7.png)

I modelli di layout consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi di applicarlo in più pagine del sito. Trovare la riga `@RenderBody()`. `RenderBody` è un segnaposto dove tutte le pagine specifiche della vista vengono presentate, &quot;incapsulate&quot; nella pagina di layout. Se ad esempio si seleziona il collegamento **About** , viene eseguito il rendering della visualizzazione *Views\Home\About.cshtml* all'interno del metodo `RenderBody`.

Modificare il contenuto dell'elemento titolo. Modificare il [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) nel modello di layout da &quot;nome dell'applicazione&quot; per &quot;&quot; di film MVC e il controller da `Home` a `Movies`. Il file di layout completo è illustrato di seguito:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Eseguire l'applicazione e notare che ora &quot;&quot;film MVC. Fare clic sul collegamento **About (informazioni** ). si vedrà come la pagina mostra anche &quot;Movie MVC&quot;. È stato possibile apportare la modifica una volta nel modello di layout e fare in modo che tutte le pagine del sito riflettano il nuovo titolo.

![](adding-a-view/_static/image8.png)

Quando è stato creato per la prima volta il file *Views\HelloWorld\Index.cshtml* , contiene il codice seguente:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Il codice Razor precedente sta impostando in modo esplicito la pagina layout. Esaminare le *visualizzazioni\\file _ViewStart. cshtml* , che contiene esattamente lo stesso markup Razor. Il file *[Views\\_ViewStart. cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* definisce il layout comune che tutte le visualizzazioni utilizzeranno, pertanto è possibile impostare come commento o rimuovere tale codice dal file *Views\HelloWorld\Index.cshtml* .

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

È possibile usare la proprietà `Layout` per impostare una vista di layout differente oppure impostarlo su `null` e quindi non verrà usato alcun file di layout.

A questo punto, modificare il titolo della visualizzazione index.

Aprire *MvcMovie\Views\HelloWorld\Index.cshtml*. Sono disponibili due posizioni per apportare una modifica: innanzitutto, il testo visualizzato nel titolo del browser e quindi nell'intestazione secondaria (elemento `<h2>`). Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Per indicare il titolo HTML da visualizzare, il codice precedente imposta una proprietà di `Title` dell'oggetto `ViewBag`, che si trova nel modello di vista *index. cshtml* . Si noti che il modello di layout ( *Views\Shared\\_Layout. cshtml* ) utilizza questo valore nell'elemento `<title>` come parte della sezione `<head>` del codice HTML modificato in precedenza.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Utilizzando questo approccio `ViewBag`, è possibile passare facilmente altri parametri tra il modello di visualizzazione e il file di layout.

Eseguire l'applicazione. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server. Il titolo del browser viene creato con i `ViewBag.Title` impostati nel modello di vista *index. cshtml* e l'app &quot;-Movie aggiuntiva&quot; aggiunta nel file di layout.

Si noti anche che il contenuto del modello di vista *index. cshtml* è stato unito al modello di vista *\_layout. cshtml* ed è stata inviata una singola risposta HTML al browser. I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.

![](adding-a-view/_static/image9.png)

Il &quot;&quot; di dati (in questo caso il &quot;Hello dal modello di visualizzazione&quot; messaggio) è tuttavia hardcoded. L'applicazione MVC ha un &quot;V&quot; (visualizzazione) e si dispone di un &quot;C&quot; (controller), ma non è ancora &quot;M&quot; (modello). A breve verrà illustrato come creare un database e recuperare i dati del modello.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e a parlare dei modelli, tuttavia, parliamo prima di passare le informazioni dal controller a una visualizzazione. Le classi controller vengono richiamate in risposta a una richiesta URL in ingresso. Una classe controller è la posizione in cui si scrive il codice che gestisce le richieste del browser in ingresso, recupera i dati da un database e infine decide il tipo di risposta da restituire al browser. I modelli di visualizzazione possono quindi essere usati da un controller per generare e formattare una risposta HTML al browser.

I controller sono responsabili di fornire tutti i dati o gli oggetti necessari per consentire a un modello di visualizzazione di eseguire il rendering di una risposta al browser. Procedura consigliata: **un modello di vista non deve mai eseguire la logica di business o interagire direttamente con un database**. Al contrario, un modello di visualizzazione dovrebbe funzionare solo con i dati forniti dal controller. Il mantenimento di questo &quot;la separazione dei problemi&quot; consente di mantenere il codice pulito, testabile e più gestibile.

Attualmente, il metodo di azione `Welcome` nella classe `HelloWorldController` accetta un `name` e un parametro `numTimes` e quindi restituisce i valori direttamente al browser. Anziché fare in modo che il controller esegua il rendering di questa risposta come stringa, modificare il controller per usare un modello di visualizzazione. Il modello di vista genererà una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta. È possibile eseguire questa operazione facendo in modo che il controller inserisca i dati dinamici (parametri) necessari per il modello di visualizzazione in un oggetto `ViewBag` a cui il modello di visualizzazione può accedere.

Tornare al file *HelloWorldController.cs* e modificare il metodo `Welcome` per aggiungere un valore `Message` e `NumTimes` all'oggetto `ViewBag`. `ViewBag` è un oggetto dinamico, ovvero è possibile inserire qualsiasi valore desiderato. l'oggetto `ViewBag` non dispone di proprietà definite fino a quando non si inserisce un elemento al suo interno. Il [sistema di associazione di modelli MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) esegue automaticamente il mapping dei parametri denominati (`name` e `numTimes`) dalla stringa di query nella barra degli indirizzi ai parametri nel metodo. Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

A questo punto l'oggetto `ViewBag` contiene i dati che verranno passati automaticamente alla visualizzazione. Quindi, è necessario un modello di visualizzazione introduttiva. Nel menu **Compila** scegliere **Compila soluzione** (o CTRL + MAIUSC + B) per assicurarsi che il progetto venga compilato. Fare clic con il pulsante destro del mouse sulla cartella *Views\HelloWorld* e scegliere **Aggiungi**, quindi fare clic su **MVC 5 Visualizza pagina con layout (Razor)** .
  
![](adding-a-view/_static/image10.png)   
  
Nella finestra di dialogo **Specifica nome per l'elemento** immettere *Welcome*, quindi fare clic su **OK**.   
  
Nella finestra di dialogo **selezionare una pagina di layout** accettare il valore predefinito **\_layout. cshtml** e fare clic su **OK**.  
  
![](adding-a-view/_static/image11.png)   

Il file *MvcMovie\Views\HelloWorld\Welcome.cshtml* viene creato.

Sostituire il markup nel file *Welcome. cshtml* . Verrà creato un ciclo che indica &quot;Hello&quot; il numero di volte indicato dall'utente. Di seguito è riportato il file completo *Welcome. cshtml* .

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Eseguire l'applicazione e passare all'URL seguente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ora i dati vengono ricavati dall'URL e passati al controller usando lo strumento di [associazione di modelli](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Il controller inserisce i dati in un oggetto `ViewBag` e passa tale oggetto alla visualizzazione. La vista Visualizza quindi i dati in formato HTML per l'utente.

![](adding-a-view/_static/image12.png)

Nell'esempio precedente è stato usato un oggetto `ViewBag` per passare i dati dal controller a una visualizzazione. Più avanti nell'esercitazione si userà un modello di vista per passare i dati da un controller a una vista. L'approccio al modello di visualizzazione per il passaggio dei dati è in genere preferibile rispetto all'approccio del contenitore di viste. Per ulteriori informazioni, vedere il post di Blog [Dynamic V](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con tipizzazione forte. 

Beh, era un tipo di &quot;M&quot; per il modello, ma non per il tipo di database. Creare un database di film con i concetti appresi.

> [!div class="step-by-step"]
> [Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)
