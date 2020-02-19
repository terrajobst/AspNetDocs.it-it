---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Utilizzo dell'helper DropDownList con ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6375bb2be158cea18309ffa71c71ac3e67bc91ed
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457869"
---
# <a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Uso dell'helper DropDownList con ASP.NET MVC

di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra le nozioni di base sull'uso dell'helper [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) e dell'helper [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) in un'applicazione Web MVC ASP.NET. È possibile utilizzare Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio per seguire l'esercitazione. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:

- [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)

Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). In questa esercitazione si presuppone che sia stata completata l'esercitazione [introduttiva a ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) o che si abbia familiarità con lo sviluppo di ASP.NET MVC. Questa esercitazione inizia con un progetto modificato dall'esercitazione su [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) . È possibile scaricare il progetto Starter con il seguente collegamento [scaricare la C# versione](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Per accompagnare questo argomento, è disponibile un C# progetto Visual Web Developer con il codice sorgente dell'esercitazione completata. [Scaricare](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Scopo dell'esercitazione

Verranno creati metodi di azione e visualizzazioni che usano l'helper [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) per selezionare una categoria. Si userà anche **jQuery** per aggiungere una finestra di dialogo Inserisci categoria che può essere usata quando è necessaria una nuova categoria, ad esempio genere o artista. Di seguito è riportata una schermata della creazione della vista che mostra i collegamenti per aggiungere un nuovo genere e aggiungere un nuovo artista.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Acquisizione di competenze

In questa esercitazione si apprenderà:

- Come utilizzare l'helper [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) per selezionare i dati di categoria.
- Come aggiungere una finestra di dialogo **jQuery** per aggiungere nuove categorie.

### <a name="getting-started"></a>Guida introduttiva

Per iniziare, scaricare il progetto Starter con il collegamento seguente, [scaricare](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). In Esplora risorse fare clic con il pulsante destro del mouse sul file *DDL\_Starter. zip* e selezionare Proprietà. Nella finestra di dialogo delle **Proprietà DDL\_Starter. zip** selezionare Sblocca.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Fare clic con il pulsante destro del mouse sul file DDL\_Starter. zip e selezionare **Estrai tutto** per decomprimere il file. Aprire il file *StartMusicStore. sln* con Visual web Developer 2010 Express ("Visual Web Developer" o "VWD" per brevità) o visual Studio 2010.

Premere CTRL + F5 per eseguire l'applicazione e fare clic sul collegamento **test** .

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Selezionare il collegamento **Seleziona categoria film (semplice)** . Viene visualizzato un elenco di selezione del tipo di film, con la commedia del valore selezionato.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Fare clic con il pulsante destro del mouse sul browser e scegliere Visualizza origine. Viene visualizzato il codice HTML per la pagina. Il codice seguente mostra il codice HTML per l'elemento SELECT.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Come si può notare, ogni elemento nell'elenco di selezione ha un valore (0 per Action, 1 per Drama, 2 per Comedy e 3 for Science Fiction) e un nome visualizzato (Action, Drama, Comedy e Science Fiction). Il codice precedente è HTML standard per un elenco di selezione.

Modificare l'elenco di selezione in Drama e premere il pulsante **Submit (Invia** ). L'URL nel browser è `http://localhost:2468/Home/CategoryChosen?MovieType=1` e viene visualizzata la pagina **selezionata: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Aprire il file *Controllers\HomeController.cs* ed esaminare il metodo `SelectCategory`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

L'helper [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) usato per creare un elenco di selezione HTML richiede un elemento **IEnumerable&lt;SelectListItem &gt;** , in modo esplicito o implicito. Ovvero, è possibile passare l'oggetto **ienumerable&lt;SelectListItem &gt;** in modo esplicito all'helper [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) oppure è possibile aggiungere **IEnumerable&lt;SelectListItem &gt;** a [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) usando lo stesso nome per **SelectListItem** come proprietà del modello. Il passaggio di **SelectListItem** in modo implicito e esplicito viene trattato nella parte successiva dell'esercitazione. Il codice precedente Mostra il modo più semplice possibile per creare un oggetto **IEnumerable&lt;SelectListItem &gt;** e popolarlo con testo e valori. Si noti che la proprietà [selezionata](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) per `Comedy`[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) è impostata su **true** . in questo modo l'elenco di selezione di cui è stato eseguito il rendering Mostra la **commedia** come elemento selezionato nell'elenco.

L'oggetto **IEnumerable&lt;SelectListItem &gt;** creato in precedenza viene aggiunto al [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con il nome MovieType. Questo è il modo in cui viene passato **IEnumerable&lt;SelectListItem &gt;** in modo implicito all'helper [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) riportato di seguito.

Aprire il file *Views\Home\SelectCategory.cshtml* ed esaminare il markup.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Nella terza riga, il layout viene impostato su Views/Shared/\_Simple\_layout. cshtml, che è una versione semplificata del file di layout standard. Questa operazione viene eseguita per semplificare la visualizzazione e il rendering del codice HTML.

In questo esempio non viene modificato lo stato dell'applicazione, quindi i dati verranno inviati tramite **http Get**, non **http post**. Vedere la sezione W3C [Quick checklist per scegliere http Get o post](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Poiché l'applicazione non viene modificata e viene pubblicato il modulo, viene usato l'overload [HTML. BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) che consente di specificare il metodo di azione, il controller e il metodo del modulo (**http post** o **http Get**). In genere le visualizzazioni contengono l'overload [HTML. BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) che non accetta parametri. Per impostazione predefinita, la versione di nessun parametro consente di pubblicare i dati del modulo nella versione successiva dello stesso metodo di azione e del controller.

Nella riga seguente

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

passa un argomento di stringa all'helper **DropDownList** . Questa stringa, "MovieType", in questo esempio, esegue due operazioni:

- Fornisce la chiave per l'helper **DropDownList** per trovare un oggetto **IEnumerable&lt;SelectListItem &gt;** in **ViewBag**.
- È associato a dati all'elemento del form MovieType. Se il metodo Submit è **http Get**, `MovieType` sarà una stringa di query. Se il metodo Submit è **http post**, `MovieType` verrà aggiunto nel corpo del messaggio. Nell'immagine seguente viene illustrata la stringa di query con il valore 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Il codice seguente illustra il metodo `CategoryChosen` a cui è stato inviato il modulo.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Tornare alla pagina test e selezionare il collegamento **HTML SELECT** . La pagina HTML esegue il rendering di un elemento SELECT simile alla semplice pagina di test di ASP.NET MVC. Fare clic con il pulsante destro del mouse sulla finestra del browser e scegliere **Visualizza origine**. Il markup HTML per l'elenco di selezione è essenzialmente identico. Testare la pagina HTML, funziona come il metodo di azione MVC ASP.NET e la vista precedentemente testata.

### <a name="improving-the-movie-select-list-with-enums"></a>Miglioramento dell'elenco di selezione dei film con enumerazioni

Se le categorie nell'applicazione sono fisse e non vengono modificate, è possibile sfruttare le enumerazioni per rendere il codice più affidabile e più semplice da estendere. Quando si aggiunge una nuova categoria, viene generato il valore corretto per la categoria. Consente di evitare gli errori di copia e incolla quando si aggiunge una nuova categoria, ma si dimentica di aggiornare il valore della categoria.

Aprire il file *Controllers\HomeController.cs* ed esaminare il codice seguente:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

Il `eMovieCategories` [enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) acquisisce i quattro tipi di film. Il metodo `SetViewBagMovieType` crea il **&gt;IEnumerable&lt;SelectListItem** dall' **enumerazione**`eMovieCategories`e imposta la proprietà `Selected` dal parametro `selectedMovie`. Il metodo di azione `SelectCategoryEnum` utilizza la stessa visualizzazione del metodo di azione `SelectCategory`.

Passare alla pagina di test e fare clic sul collegamento `Select Movie Category (Enum)`. Questa volta, invece di un valore (numero) visualizzato, viene visualizzata una stringa che rappresenta l'enumerazione.

### <a name="posting-enum-values"></a>Pubblicazione di valori enum

I moduli HTML vengono in genere utilizzati per inviare dati al server. Il codice seguente illustra le versioni `HTTP GET` e `HTTP POST` del metodo `SelectCategoryEnumPost`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Passando un `eMovieCategories` enum al metodo `POST`, è possibile estrarre sia il valore enum che la stringa enum. Eseguire l'esempio e passare alla pagina di test. Fare clic sul collegamento `Select Movie Category(Enum Post)`. Selezionare un tipo di film e quindi fare clic sul pulsante Submit (Invia). La visualizzazione Mostra sia il valore che il nome del tipo di film.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Creazione di un elemento SELECT per più sezioni

L'helper HTML [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) esegue il rendering dell'elemento HTML `<select>` con l'attributo `multiple`, che consente agli utenti di effettuare più selezioni. Passare al collegamento test, quindi selezionare il collegamento **MultiSelect Country** . L'interfaccia utente di cui è stato eseguito il rendering consente di selezionare più paesi. Nell'immagine seguente sono selezionati Canada e Cina.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Esame del codice MultiSelectCountry

Esaminare il codice seguente dal file *Controllers\HomeController.cs* .

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

Il metodo `GetCountries` crea un elenco di paesi, quindi lo passa al costruttore di `MultiSelectList`. L'overload del costruttore `MultiSelectList` usato nel metodo `GetCountries` precedente accetta quattro parametri:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *Items*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenente gli elementi dell'elenco. Nell'esempio precedente, l'elenco dei paesi.
2. *dataValueField*: nome della proprietà nell'elenco **IEnumerable** che contiene il valore. Nell'esempio precedente, la proprietà `ID`.
3. *TextField*: nome della proprietà nell'elenco **IEnumerable** che contiene le informazioni da visualizzare. Nell'esempio precedente, la proprietà `name`.
4. *SelectedValues*: elenco di valori selezionati.

Nell'esempio precedente, il metodo `MultiSelectCountry` passa un valore `null` per i paesi selezionati, quindi non viene selezionato alcun paese quando viene visualizzata l'interfaccia utente. Il codice seguente illustra il markup Razor usato per eseguire il rendering della visualizzazione `MultiSelectCountry`.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Il metodo [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) helper HTML usato sopra accetta due parametri, il nome della proprietà da associare al modello e l'oggetto [MultiSelect](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) contenente le opzioni e i valori selezionati. Il codice `ViewBag.YouSelected` sopra riportato viene usato per visualizzare i valori dei paesi selezionati quando si invia il modulo. Esaminare l'overload HTTP POST del metodo `MultiSelectCountry`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

La proprietà dinamica `ViewBag.YouSelected` contiene i paesi selezionati, ottenuti per la voce `Countries` nella raccolta di form. In questa versione al metodo getpaesi viene passato un elenco dei paesi selezionati, quindi quando viene visualizzata la visualizzazione `MultiSelectCountry`, i paesi selezionati vengono selezionati nell'interfaccia utente.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Creazione di un elemento SELECT descrittivo con il plug-in jQuery scelto

È possibile aggiungere il plug-in jQuery [scelto](https://harvesthq.github.com/chosen/) a un HTML &lt;selezionare&gt; elemento per creare un'interfaccia utente intuitiva. Le immagini seguenti illustrano il plug-in jQuery [scelto](https://harvesthq.github.com/chosen/) con `MultiSelectCountry` visualizzazione.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

Nelle due immagini seguenti è selezionato **Canada** .

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Nell'immagine precedente, il Canada è selezionato e contiene una **x** che è possibile fare clic per rimuovere la selezione. L'immagine seguente mostra la selezione di Canada, Cina e Giappone.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Aggancio della raccolta con il plug-in jQuery scelto

La sezione seguente è più semplice da seguire se si ha esperienza con jQuery. Se jQuery non è mai stato usato in precedenza, è consigliabile provare una delle esercitazioni di jQuery seguenti.

- Funzionamento di [jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) di [John Resig](http://ejohn.org/)
- [Introduzione con jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) di [Jörn Zaefferer](http://bassistance.de/)
- [Esempi dinamici di jQuery](http://codylindley.com/blogstuff/js/jquery/#) di [Cody Lindley](http://codylindley.com/)

Il plug-in scelto è incluso nei progetti di esempio Starter e Completed che accompagnano questa esercitazione. Per questa esercitazione sarà necessario usare jQuery solo per associarlo all'interfaccia utente. Per usare il plug-in jQuery scelto in un progetto MVC ASP.NET, è necessario:

1. Scaricare il plug-in selezionato da [GitHub](https://github.com/harvesthq/chosen/). Questo passaggio è stato eseguito per l'utente.
2. Aggiungere la cartella scelta al progetto MVC ASP.NET. Aggiungere gli asset dal plug-in scelto scaricato nel passaggio precedente alla cartella scelta. Questo passaggio è stato eseguito per l'utente.
3. Associare il plug-in scelto all'helper HTML **DropDownList** o **ListBox** .

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Associare il plug-in scelto alla visualizzazione MultiSelectCountry.

Aprire il file *Views\Home\MultiSelectCountry.cshtml* e aggiungere un parametro `htmlAttributes` al `Html.ListBox`. Il parametro che si aggiunge conterrà un nome di classe per l'elenco di selezione (`@class = "chzn-select"`). Il codice completato è illustrato di seguito:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Nel codice precedente vengono aggiunti l'attributo HTML e il valore dell'attributo `class = "chzn-select"`. Il carattere \@ precedente la classe non ha nulla a che fare con il motore di visualizzazione Razor. `class` è una [ C# parola chiave](https://msdn.microsoft.com/library/x53a06bb.aspx). C#le parole chiave non possono essere usate come identificatori a meno che non includano \@ come prefisso. Nell'esempio precedente, `@class` è un identificatore valido, ma la **classe** non è perché la **classe** è una parola chiave.

Aggiungere i riferimenti ai file CSS scelti/scelti. *jQuery. js* e *scelti/scelti* . L'oggetto *scelto/scelto. jQuery. js* e implementa la funzione del plug-in scelto. Il file *CSS scelto/scelto* fornisce lo stile. Aggiungere questi riferimenti alla fine del file *Views\Home\MultiSelectCountry.cshtml* . Il codice seguente illustra come fare riferimento al plug-in scelto.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Attivare il plug-in scelto usando il nome della classe usato nel codice **HTML. ListBox** . Nell'esempio precedente, il nome della classe è `chzn-select`. Aggiungere la riga seguente alla fine del file di visualizzazione *Views\Home\MultiSelectCountry.cshtml* . Questa riga attiva il plug-in scelto.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

La riga seguente è la sintassi per chiamare la funzione jQuery Ready, che seleziona l'elemento DOM con il nome della classe `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Il set di cui è stato eseguito il wrapped restituito dalla chiamata precedente applica quindi il metodo scelto (`.chosen();`), che associa il plug-in scelto.

Il codice seguente illustra il file di visualizzazione *Views\Home\MultiSelectCountry.cshtml* completato.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Eseguire l'applicazione e passare alla visualizzazione `MultiSelectCountry`. Provare ad aggiungere ed eliminare paesi. Il download di esempio fornito contiene anche un metodo `MultiCountryVM` e una vista che implementa la funzionalità MultiSelectCountry usando un modello di visualizzazione anziché un **ViewBag**.

Nella sezione successiva si vedrà come funziona il meccanismo di impalcatura MVC ASP.NET con l'helper **DropDownList** .

> [!div class="step-by-step"]
> [avanti](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
