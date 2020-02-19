---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Uso del calendario popup per HTML5 e jQuery UI DatePicker con ASP.NET MVC-parte 4 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il calendario popup DatePicker dell'interfaccia utente di jQuery in un ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 583e782641efea9a9517edb31f7718b28203d756
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457492"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Uso del calendario popup per HTML5 e jQuery UI DatePicker con ASP.NET MVC-parte 4

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il calendario popup DatePicker dell'interfaccia utente di jQuery in un'applicazione Web MVC ASP.NET.

### <a name="adding-a-template-for-editing-dates"></a>Aggiunta di un modello per la modifica delle date

In questa sezione verrà creato un modello per modificare le date da applicare quando ASP.NET MVC Visualizza l'interfaccia utente per la modifica delle proprietà del modello contrassegnate con l'enumerazione **date** dell'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) . Il modello eseguirà il rendering solo della data; l'ora non verrà visualizzata. Nel modello verrà usato il calendario popup [DatePicker interfaccia utente jQuery](http://jqueryui.com/demos/datepicker/) per fornire un modo per modificare le date.

Per iniziare, aprire il file *Movie.cs* e aggiungere l'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) con l'enumerazione **date** alla proprietà `ReleaseDate`, come illustrato nel codice seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Questo codice fa in modo che il campo `ReleaseDate` venga visualizzato senza l'ora sia nei modelli di visualizzazione che nei modelli di modifica. Se l'applicazione contiene un modello *date. cshtml* nella cartella *Views\Shared\EditorTemplates* o nella cartella *Views\Movies\EditorTemplates* , il modello verrà usato per eseguire il rendering di qualsiasi proprietà `DateTime` durante la modifica. In caso contrario, il sistema di modelli ASP.NET incorporato visualizzerà la proprietà come data.

Premere CTRL+F5 per eseguire l'applicazione. Selezionare un collegamento modifica per verificare che il campo di input per la data di rilascio mostri solo la data.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

In **Esplora soluzioni**espandere la cartella *views* , espandere la cartella *Shared* , quindi fare clic con il pulsante destro del mouse sulla cartella *Views\Shared\EditorTemplates* .

Fare clic su **Aggiungi**e quindi su **Visualizza**. Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** .

Nella casella **nome visualizzazione** Digitare &quot;data&quot;.

Selezionare la casella **di controllo Crea come visualizzazione parziale** . Verificare che le caselle di controllo **Usa un layout o una pagina master** e **Crea una visualizzazione fortemente tipizzata** non siano selezionate.

Fare clic su **Add**. Viene creato il modello *Views\Shared\EditorTemplates\Date.cshtml* .

Aggiungere il codice seguente al modello *Views\Shared\EditorTemplates\Date.cshtml* .

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

La prima riga dichiara il modello come tipo di `DateTime`. Sebbene non sia necessario dichiarare il tipo di modello nei modelli di modifica e visualizzazione, è una procedura consigliata per ottenere il controllo in fase di compilazione del modello passato alla visualizzazione. Un altro vantaggio è la possibilità di ottenere IntelliSense per il modello nella visualizzazione in Visual Studio. Se il tipo di modello non è dichiarato, ASP.NET MVC lo considera un tipo [dinamico](https://msdn.microsoft.com/library/dd264741.aspx) e non esiste alcun controllo del tipo in fase di compilazione. Se si dichiara il modello come tipo di `DateTime`, diventa fortemente tipizzato.

La seconda riga è semplicemente markup HTML letterale che visualizza &quot;utilizzando il modello di data&quot; prima di un campo di data. Questa riga verrà utilizzata temporaneamente per verificare che sia in uso questo modello di data.

La riga successiva è un helper [HTML. TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) che esegue il rendering di un campo `input` che è una casella di testo. Il terzo parametro per l'helper utilizza un tipo anonimo per impostare la classe per la casella di testo `datefield` e il tipo su `date`. Poiché `class` è un riservato in C#, è necessario utilizzare il carattere `@` per eseguire l'escape dell'attributo `class` nel C# parser.

Il tipo di `date` è un tipo di input HTML5 che consente ai browser compatibili con HTML5 di eseguire il rendering di un controllo calendario HTML5. Successivamente, si aggiungerà un codice JavaScript per associare l'oggetto DatePicker di jQuery all'elemento `Html.TextBox` usando la classe `datefield`.

Premere CTRL+F5 per eseguire l'applicazione. È possibile verificare che la proprietà `ReleaseDate` nella visualizzazione di modifica usi il modello di modifica perché il modello Visualizza &quot;utilizzando il modello di data&quot; immediatamente prima della casella di input di testo `ReleaseDate`, come illustrato nell'immagine seguente:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Nel browser visualizzare l'origine della pagina. Ad esempio, fare clic con il pulsante destro del mouse sulla pagina e scegliere **Visualizza origine**. Nell'esempio seguente viene illustrato il markup per la pagina, illustrando gli attributi `class` e `type` nel codice HTML sottoposto a rendering.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Tornare al modello *Views\Shared\EditorTemplates\Date.cshtml* e rimuovere il &quot;usando il modello di data&quot; markup. Il modello completato è ora simile al seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Aggiunta di un calendario popup DatePicker interfaccia utente jQuery con NuGet

In questa sezione verrà aggiunto il calendario popup dell' [interfaccia utente di jQuery](http://jqueryui.com/demos/datepicker/) al modello Data-modifica. La libreria [dell'interfaccia utente di jQuery](http://jqueryui.com/) fornisce supporto per animazioni, effetti avanzati e widget personalizzabili. Si basa sulla libreria JavaScript jQuery. Il calendario popup DatePicker rende semplice e naturale immettere le date usando un calendario anziché immettere una stringa. Il calendario popup limita anche gli utenti alle date legali. la voce di testo ordinaria per una data consente di immettere un valore come `2/33/1999` (il 33 febbraio 1999), ma il calendario popup dell' [interfaccia utente jQuery](http://jqueryui.com/demos/datepicker/) popup non lo consentirà.

Prima di tutto, è necessario installare le librerie dell'interfaccia utente di jQuery. A tale scopo, si userà NuGet, ovvero una gestione pacchetti inclusa nelle versioni SP1 di Visual Studio 2010 e Visual Web Developer.

In Visual Web Developer scegliere **Gestione pacchetti NuGet** dal menu **strumenti** e quindi selezionare **Gestisci pacchetti NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Nota: se il menu **strumenti** non Visualizza il comando **Gestione pacchetti NuGet** , è necessario installare NuGet seguendo le istruzioni disponibili nella pagina [installazione di NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) del sito Web NuGet.   
  
Se si usa Visual Studio anziché Visual Web Developer, scegliere **Gestione pacchetti NuGet** dal menu **strumenti** e quindi fare clic su **Aggiungi riferimento al pacchetto di librerie**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

Nella finestra di dialogo **MVCMovie-Gestisci pacchetti NuGet** fare clic sulla scheda **online** a sinistra e quindi immettere &quot;jQuery. UI&quot; nella casella di ricerca. Selezionare il **widget interfaccia utente query j: DatePicker**, quindi selezionare il pulsante **Installa** .

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet aggiunge le versioni di debug e le versioni minimizzati di jQuery UI Core e il selettore di data UI jQuery per il progetto:

- *jQuery. UI. Core. js*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. js*
- *jQuery. UI. DatePicker. min. js*

Nota: le versioni di debug (i file senza estensione *. min. js* ) sono utili per il debug, ma in un sito di produzione sono incluse solo le versioni minimizzati.

Per usare effettivamente la selezione data di jQuery, è necessario creare uno script jQuery che collegherà il widget Calendar al modello di modifica. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *script* , scegliere **Aggiungi**, quindi **nuovo elemento**e quindi **file JScript**. Denominare il file *DatePickerReady. js*.

Aggiungere il codice seguente al file *DatePickerReady. js* :

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Se non si ha familiarità con jQuery, ecco una breve spiegazione di ciò che accade: la prima riga è la &quot;funzione jQuery Ready&quot;, che viene chiamata quando tutti gli elementi DOM in una pagina sono stati caricati. La seconda riga Seleziona tutti gli elementi DOM con il nome della classe `datefield`, quindi richiama la funzione `datepicker` per ognuno di essi. Tenere presente che è stata aggiunta la classe `datefield` al modello *Views\Shared\EditorTemplates\Date.cshtml* in precedenza nell'esercitazione.

Aprire quindi il file *Views\Shared\\_Layout. cshtml* . È necessario aggiungere i riferimenti ai file seguenti, che sono tutti necessari per poter usare il selettore di data:

- *Content/themes/base/jQuery. UI. Core. CSS*
- *Content/themes/base/jQuery. UI. DatePicker. CSS*
- *Content/themes/base/jQuery. UI. Theme. CSS*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. min. js*
- *DatePickerReady. js*

Nell'esempio seguente viene illustrato il codice effettivo che è necessario aggiungere nella parte inferiore dell'elemento `head` nel file *Views\Shared\\_Layout. cshtml* .

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

La sezione `head` completa è illustrata di seguito:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

Il metodo di [supporto del contenuto dell'URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) converte il percorso della risorsa in un percorso assoluto. È necessario utilizzare `@URL.Content` per fare riferimento a queste risorse quando l'applicazione è in esecuzione in IIS.

Premere CTRL+F5 per eseguire l'applicazione. Selezionare un collegamento modifica, quindi inserire il punto di inserimento nel campo **rilasciato** . Viene visualizzato il calendario popup dell'interfaccia utente di jQuery.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Come la maggior parte dei controlli jQuery, DatePicker consente di personalizzarlo in modo esteso. Per informazioni, vedere [personalizzazione visiva: progettazione di un tema dell'interfaccia utente jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) sul sito dell' [interfaccia utente di jQuery](http://learn.jquery.com/jquery-ui/getting-started/) .

### <a name="supporting-the-html5-date-input-control"></a>Supporto del controllo di input di data HTML5

Poiché più browser supportano HTML5, è opportuno usare l'input HTML5 nativo, ad esempio l'elemento `date` input, e non usare il calendario dell'interfaccia utente di jQuery. È possibile aggiungere la logica all'applicazione per usare automaticamente i controlli HTML5 se supportati dal browser. A tale scopo, sostituire il contenuto del file *DatePickerReady. js* con il codice seguente:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

La prima riga di questo script usa Modernizzator per verificare che l'input della data HTML5 sia supportato. Se non è supportato, il selettore di data dell'interfaccia utente jQuery viene invece collegato. [Modernizzator](http://www.modernizr.com/docs/) è una libreria JavaScript open source che rileva la disponibilità di implementazioni native di HTML5 e CSS3. Modernizzator è incluso in tutti i nuovi progetti MVC ASP.NET creati.

Dopo aver apportato questa modifica, è possibile testarla usando un browser che supporta HTML5, ad esempio opera 11. Eseguire l'applicazione usando un browser compatibile con HTML5 e modificare una voce di film. Il controllo della data HTML5 viene usato al posto del calendario popup dell'interfaccia utente di jQuery:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Poiché le nuove versioni dei browser implementano HTML5 in modo incrementale, un approccio efficace per ora consiste nell'aggiungere codice al sito Web che include una vasta gamma di supporto per HTML5. Ad esempio, di seguito è riportato uno script *DatePickerReady. js* più solido che consente al sito di supportare i browser che supportano solo parzialmente il controllo della data HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Questo script Seleziona gli elementi `input` HTML5 di tipo `date` che non supportano completamente il controllo della data HTML5. Per tali elementi, il calendario popup dell'interfaccia utente jQuery viene collegato e quindi l'attributo `type` viene modificato da `date` a `text`. Modificando l'attributo `type` da `date` a `text`, viene eliminato il supporto della data HTML5 parziale. Uno script *DatePickerReady. js* ancora più affidabile si trova in [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Aggiunta di date nullable ai modelli

Se si usa uno dei modelli di data esistenti e si passa una data null, si otterrà un errore di run-time. Per rendere più affidabili i modelli di data, è necessario modificarli per gestire i valori null. Per supportare le date nullable, modificare il codice in *Views\Shared\DisplayTemplates\DateTime.cshtml* nel modo seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Il codice restituisce una stringa vuota quando il modello è **null**.

Modificare il codice nel file *Views\Shared\EditorTemplates\Date.cshtml* nel modo seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Quando viene eseguito questo codice, se il modello non è null, viene utilizzato il valore `DateTime` del modello. Se il modello è null, viene invece utilizzata la data corrente.

### <a name="wrapup"></a>Wrapup

Questa esercitazione ha illustrato le nozioni di base degli helper basati su modelli di ASP.NET e illustra come usare il calendario popup di DatePicker interfaccia utente jQuery in un'applicazione MVC ASP.NET. Per ulteriori informazioni, provare a usare le risorse seguenti:

- Per informazioni sulla localizzazione, vedere il Blog di [jQueryUI DatePicker in ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Per informazioni sull'interfaccia utente di jQuery, vedere [interfaccia utente di jQuery](http://docs.jquery.com/UI).
- Per informazioni su come localizzare il controllo DatePicker, vedere [UI/DatePicker/Localization](http://docs.jquery.com/UI/Datepicker/Localization).
- Per ulteriori informazioni sui modelli MVC ASP.NET, vedere la serie di Blog di Brad Wilson sui [modelli ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Sebbene la serie sia stata scritta per ASP.NET MVC 2, il materiale è ancora applicabile alla versione corrente di ASP.NET MVC.

> [!div class="step-by-step"]
> [Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
