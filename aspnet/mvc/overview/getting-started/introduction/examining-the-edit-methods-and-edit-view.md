---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Analisi dei metodi di modifica e della visualizzazione di modifica | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 6cef963910b957e8b4ad7c7909385f6dbdff95c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582439"
---
# <a name="examining-the-edit-methods-and-edit-view"></a>Analisi dei metodi di modifica e della visualizzazione di modifica

di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

In questa sezione verranno esaminati i metodi e le visualizzazioni di azione `Edit` generati per il controller di film. Prima di tutto, tuttavia, verrà eseguita una diversione breve per ottenere una data di rilascio migliore. Aprire il file *Models\Movie.cs* e aggiungere le righe evidenziate illustrate di seguito:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

È anche possibile rendere le impostazioni cultura della data specifiche come la seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Gli attributi [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) verranno esaminati nell'esercitazione successiva. L'attributo [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) specifica il testo da visualizzare per il nome di un campo, in questo caso "Release Date" anziché "ReleaseDate". L'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) specifica il tipo di dati, in questo caso è una data, quindi le informazioni sull'ora archiviate nel campo non vengono visualizzate. L'attributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) è necessario per un bug nel browser Chrome che esegue il rendering dei formati di data in modo errato.

Eseguire l'applicazione e passare al controller di `Movies`. Mantenere il puntatore del mouse su un collegamento **Edit (modifica** ) per visualizzare l'URL a cui si collega.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Il collegamento di **modifica** è stato generato dal metodo `Html.ActionLink` nella vista *Views\Movies\Index.cshtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![HTML. ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

L'oggetto `Html` è un helper esposto usando una proprietà nella classe di base [System. Web. Mvc. WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) . Il metodo `ActionLink` dell'helper semplifica la generazione dinamica di collegamenti ipertestuali HTML che si collegano ai metodi di azione nei controller. Il primo argomento del metodo `ActionLink` è il testo del collegamento di cui eseguire il rendering, ad esempio `<a>Edit Me</a>`. Il secondo argomento è il nome del metodo di azione da richiamare (in questo caso, l'azione `Edit`). L'argomento finale è un [oggetto anonimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) che genera i dati della route (in questo caso, l'ID 4).

Il collegamento generato illustrato nell'immagine precedente è `http://localhost:1234/Movies/Edit/4`. La route predefinita (stabilita nell' *App\_Start\RouteConfig.cs*) accetta il modello di URL `{controller}/{action}/{id}`. Pertanto, ASP.NET converte `http://localhost:1234/Movies/Edit/4` in una richiesta al metodo di azione `Edit` del controller `Movies` con il parametro `ID` uguale a 4. Esaminare il codice seguente dall' *App\_file Start\RouteConfig.cs* . Il metodo [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) viene usato per instradare le richieste HTTP al metodo corretto del controller e dell'azione e fornire il parametro ID facoltativo. Il metodo [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) viene usato anche dalle [htmlHelper](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) , ad esempio `ActionLink` per generare URL, dato il controller, il metodo di azione e tutti i dati della route.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

È anche possibile passare i parametri del metodo di azione usando una stringa di query. Ad esempio, l'URL `http://localhost:1234/Movies/Edit?ID=3` inoltre passa il parametro `ID` di 3 al metodo di azione `Edit` del controller `Movies`.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Aprire il controller di `Movies`. Di seguito sono riportati i due metodi di azione `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Si noti che il secondo metodo di azione `Edit` è preceduto dall'attributo `HttpPost`. Questo attributo specifica che l'overload del metodo `Edit` può essere richiamato solo per le richieste POST. È possibile applicare l'attributo `HttpGet` al primo metodo di modifica, ma ciò non è necessario perché è l'impostazione predefinita. Si farà riferimento ai metodi di azione a cui viene assegnato in modo implicito l'attributo `HttpGet` come `HttpGet` metodi. L'attributo [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) è un altro meccanismo di sicurezza importante che impedisce ai pirati informatici di inviare dati al modello. È necessario includere solo le proprietà nell'attributo binding che si desidera modificare. Per informazioni sull'overposting e sull'attributo bind, vedere la [Nota sulla sicurezza di overposting](https://go.microsoft.com/fwlink/?LinkId=317598). Nel modello semplice utilizzato in questa esercitazione verranno rilegati tutti i dati del modello. L'attributo [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) viene usato per impedire la falsificazione di una richiesta e viene abbinato a `@Html.AntiForgeryToken()` nel file di visualizzazione di modifica (*Views\Movies\Edit.cshtml*), una parte viene mostrata di seguito:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` genera un token antifalsificazione di form nascosto che deve corrispondere nel metodo di `Edit` del controller di `Movies`. Per altre informazioni su richiesta tra siti falsa (nota anche come XSRF o CSRF), vedere l'esercitazione [XSRF/CSRF Prevention in MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

Il `HttpGet` `Edit` metodo accetta il parametro Movie ID, Cerca il film usando il metodo Entity Framework `Find` e restituisce il filmato selezionato alla visualizzazione di modifica. Se non è possibile trovare un film, viene restituito [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) . Quando il sistema di scaffolding ha creato la vista Edit, ha esaminato la classe `Movie` e il codice creato per eseguire il rendering degli elementi `<label>` e `<input>` per ogni proprietà della classe. Nell'esempio seguente viene illustrata la visualizzazione di modifica generata dal sistema di ponteggi di Visual Studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Si noti che il modello di vista dispone di un'istruzione `@model MvcMovie.Models.Movie` all'inizio del file. in questo modo si specifica che la vista prevede che il modello per il modello di vista sia di tipo `Movie`.

Il codice con impalcature usa diversi *metodi helper* per semplificare il markup HTML. Il [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Helper Visualizza il nome del campo (&quot;titolo&quot;, &quot;rilasciata&quot;, &quot;genere&quot;o &quot;Price&quot;). Il [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Helper esegue il rendering di un elemento HTML `<input>`. L'helper [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Visualizza tutti i messaggi di convalida associati a tale proprietà.

Eseguire l'applicazione e passare all'URL */Movies* . Fare clic su un collegamento **Edit** (Modifica). Nel browser visualizzare l'origine per la pagina. Il codice HTML per l'elemento del form è illustrato di seguito.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

Gli elementi `<input>` si trovano in un elemento HTML `<form>` il cui attributo `action` è impostato su post sull'URL */Movies/Edit* . I dati del modulo verranno inviati al server quando si fa clic sul pulsante **Salva** . La seconda riga indica il token [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) nascosto generato dalla chiamata `@Html.AntiForgeryToken()`.

## <a name="processing-the-post-request"></a>Elaborazione della richiesta POST

Nell'elenco seguente viene indicata la versione `HttpPost` del metodo di azione `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

L'attributo [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) convalida il token [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) generato dalla chiamata `@Html.AntiForgeryToken()` nella visualizzazione.

Lo strumento di [associazione di modelli MVC ASP.NET](https://msdn.microsoft.com/library/dd410405.aspx) accetta i valori del modulo inviati e crea un `Movie` oggetto passato come parametro di `movie`. Il `ModelState.IsValid` verifica che i dati inviati nel modulo possano essere utilizzati per modificare (modificare o aggiornare) un oggetto `Movie`. Se i dati sono validi, i dati dei film vengono salvati nella raccolta `Movies` della `db`(`MovieDBContext` istanza). I nuovi dati del film vengono salvati nel database chiamando il metodo di `SaveChanges` `MovieDBContext`. Dopo avere salvato i dati, il codice reindirizza l'utente al metodo di azione `Index` della classe `MoviesController`, che visualizza la raccolta di film, incluse le modifiche appena apportate.

Non appena la convalida lato client determina che il valore di un campo non è valido, viene visualizzato un messaggio di errore. Se JavaScript è disabilitato, la convalida lato client è disabilitata. Tuttavia, il server rileva che i valori inviati non sono validi e i valori del modulo vengono visualizzati nuovamente con messaggi di errore.

La convalida viene esaminata in modo più dettagliato più avanti nell'esercitazione.

Gli helper `Html.ValidationMessageFor` nel modello di vista *Edit. cshtml* si occupano della visualizzazione dei messaggi di errore appropriati.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Tutti i metodi di `HttpGet` seguono un modello simile. Ottengono un oggetto Movie (o un elenco di oggetti, nel caso di `Index`) e passano il modello alla visualizzazione. Il metodo `Create` passa un oggetto Movie vuoto alla visualizzazione create. Tutti i metodi che creano, modificano, eliminano o cambiano in altro modo i dati, eseguono questa operazione nell'overload `HttpPost` del metodo. La modifica dei dati in un metodo HTTP GET costituisce un rischio per la sicurezza, come descritto nel post di Blog relativo all'introduzione a [ASP.NET MVC Tip #46 – non usare i collegamenti Delete perché creano buchi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). La modifica dei dati in un metodo GET viola anche le procedure consigliate HTTP e il modello [Rest](http://en.wikipedia.org/wiki/Representational_State_Transfer) architetturale, che specifica che le richieste Get non devono modificare lo stato dell'applicazione. In altre parole, l'esecuzione di un'operazione GET deve essere sicura, senza effetti collaterali e non modificare dati persistenti.

## <a name="jquery-validation-for-non-english-locales"></a>convalida di jQuery per le impostazioni locali diverse dall'inglese

Se si usa un computer con lingua inglese (Stati Uniti), è possibile ignorare questa sezione e passare all'esercitazione successiva. È possibile scaricare la versione globalizzata di questa esercitazione [qui](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Per un'eccellente esercitazione in due parti sull'internazionalizzazione, vedere l' [internazionalizzazione di ASP.NET MVC 5 di nero](http://afana.me/post/aspnet-mvc-internationalization.aspx).

> [!NOTE]
> per supportare la convalida jQuery per le impostazioni locali diverse dall'inglese che usano una virgola (&quot;,&quot;) per un separatore decimale e formati di data non in inglese (Stati Uniti), è necessario includere *globalizzate. js* e il file *Cultures/globalizzate. culture. js* specifico (da [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript per usare `Globalize.parseFloat`. È possibile ottenere la convalida jQuery non in lingua inglese da NuGet. (Non installare globalizzate se si usano impostazioni locali per l'inglese).

1. Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet**e quindi fare clic su **Gestisci pacchetti NuGet per la soluzione**.

    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. Nel riquadro sinistro selezionare <strong>Sfoglia *.</strong>*  (Vedere l'immagine seguente).
3. Nella casella di input immettere * globalizzate * *.

    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) scegliere `jQuery.Validation.Globalize`, scegliere `MvcMovie`, quindi fare clic su **Installa**. Il file *Scripts\jquery.globalize\globalize.js* verrà aggiunto al progetto. La cartella * Scripts\jquery.globalize\cultures\* conterrà molti file JavaScript delle impostazioni cultura. Si noti che l'installazione di questo pacchetto potrebbe richiedere cinque minuti.

   Il codice seguente mostra le modifiche apportate al file Views\Movies\Edit.cshtml:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Per evitare di ripetere questo codice in ogni visualizzazione di modifica, è possibile spostarlo nel file di layout. Per ottimizzare il download dello script, vedere la pagina relativa alla creazione di [bundle e minification](../../performance/bundling-and-minification.md)dell'esercitazione.

Per ulteriori informazioni, vedere [ASP.NET MVC 3 internazionalizzazione](http://afana.me/post/aspnet-mvc-internationalization.aspx) e [ASP.NET MVC 3 internazionalizzazione-parte 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Come correzione temporanea, se non è possibile ottenere la convalida con le impostazioni locali, è possibile forzare il computer a usare l'inglese (Stati Uniti) oppure è possibile disabilitare JavaScript nel browser. Per forzare il computer a usare l'inglese (Stati Uniti), è possibile aggiungere l'elemento di globalizzazione al file *Web. config* radice dei progetti. Il codice seguente illustra l'elemento di globalizzazione con le impostazioni cultura impostate per Stati Uniti inglese.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a>Nell'esercitazione successiva verrà implementata la funzionalità di ricerca.

> [!div class="step-by-step"]
> [Precedente](accessing-your-models-data-from-a-controller.md)
> [Successivo](adding-search.md)
