---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Aggiunta della convalida | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: f508d9e38dab5cc4cc44cc5aaa4eae87cf273bd5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615724"
---
# <a name="adding-validation"></a>Aggiunta della convalida

di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

In questa sezione si aggiungerà la logica di convalida al modello di `Movie` e si verificherà che le regole di convalida vengono applicate ogni volta che un utente tenta di creare o modificare un film usando l'applicazione.

## <a name="keeping-things-dry"></a>Conservazione degli elementi

Uno dei principi di base della progettazione di ASP.NET MVC è [Dry](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;Don't Repeat Yourself&quot;). ASP.NET MVC consiglia di specificare la funzionalità o il comportamento solo una volta e quindi di rifletterla ovunque in un'applicazione. In questo modo si riduce la quantità di codice che è necessario scrivere e il codice che si scrive meno soggetto a errori e si semplifica la manutenzione.

Il supporto della convalida fornito da ASP.NET MVC e Entity Framework Code First è un ottimo esempio di principio secco in azione. È possibile specificare in modo dichiarativo le regole di convalida in un'unica posizione (nella classe del modello) e le regole vengono applicate ovunque nell'applicazione.

Verrà ora esaminato come sfruttare questo supporto per la convalida nell'applicazione Movie.

## <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida al modello di film

Per iniziare, aggiungere una logica di convalida alla classe `Movie`.

Aprire il file *Movie.cs*. Si noti che lo spazio dei nomi [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) non contiene `System.Web`. DataAnnotations fornisce un set predefinito di attributi di convalida che è possibile applicare in modo dichiarativo a qualsiasi classe o proprietà. Contiene anche attributi di formattazione come [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) che semplificano la formattazione e non forniscono alcuna convalida.

A questo punto, aggiornare la classe `Movie` per sfruttare gli attributi di convalida predefiniti [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)e [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) . Sostituire la classe `Movie` con la seguente:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

L'attributo [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) imposta la lunghezza massima della stringa e imposta questa limitazione sul database, pertanto lo schema del database verrà modificato. Fare clic con il pulsante destro del mouse sulla tabella **Movies** in **Esplora server** e scegliere **Apri definizione tabella**:

![](adding-validation/_static/image1.png)

Nell'immagine precedente è possibile vedere che tutti i campi stringa sono impostati su [nvarchar (max)](https://technet.microsoft.com/library/ms186939.aspx). Per aggiornare lo schema, si utilizzeranno le migrazioni. Compilare la soluzione, quindi aprire la finestra **console di gestione pacchetti** e immettere i comandi seguenti:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova classe derivata `DbMigration` con il nome specificato (`DataAnnotations`) e nel metodo `Up` è possibile visualizzare il codice che aggiorna i vincoli dello schema:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

Il campo `Genre` non è più Nullable (ovvero è necessario immettere un valore). Il campo `Rating` ha una lunghezza massima di 5 e `Title` ha una lunghezza massima di 60. La lunghezza minima di 3 in `Title` e l'intervallo in `Price` non ha creato modifiche dello schema.

Esaminare lo schema del film:

![](adding-validation/_static/image2.png)

I campi stringa mostrano i nuovi limiti di lunghezza e `Genre` non viene più controllata come Nullable.

Gli attributi di convalida specificano il comportamento da applicare per le proprietà del modello a cui vengono applicati. Gli attributi `Required` e `MinimumLength` indicano che una proprietà deve avere un valore, ma nulla impedisce all'utente di inserire spazi vuoti per soddisfare questa convalida. L'attributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) viene usato per limitare i caratteri che possono essere di input. Nel codice precedente, per `Genre` e `Rating` è necessario usare solo lettere (spazi, numeri e caratteri speciali non consentiti). L'attributo [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) vincola un valore all'interno di un intervallo specificato. L'attributo `StringLength` consente di impostare la lunghezza massima di una proprietà stringa e, facoltativamente, la lunghezza minima. I tipi di valore (ad esempio `decimal, int, float, DateTime`) sono intrinsecamente necessari e non richiedono l'attributo `Required`.

Code First garantisce che le regole di convalida specificate in una classe del modello vengano applicate prima che l'applicazione salvi le modifiche nel database. Il codice seguente, ad esempio, genererà un'eccezione [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) quando viene chiamato il metodo `SaveChanges`, perché mancano alcuni valori di proprietà `Movie` obbligatori:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Il codice precedente genera l'eccezione seguente:

*Convalida non riuscita per una o più entità. Per ulteriori informazioni, vedere la proprietà' EntityValidationErrors '.*

La presenza di regole di convalida applicate automaticamente dall'.NET Framework consente di rendere l'applicazione più affidabile. In questo modo inoltre non è possibile omettere la convalida di un elemento e quindi inserire involontariamente dati errati nel database.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interfaccia utente di errore di convalida in ASP.NET MVC

Eseguire l'applicazione e passare all'URL */Movies* .

Fare clic sul collegamento **Crea nuovo** per aggiungere un nuovo film. Completare il modulo con alcuni valori non validi. Non appena la convalida del lato client jQuery rileva l'errore, viene visualizzato un messaggio di errore.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> per supportare la convalida jQuery per le impostazioni locali diverse dall'inglese che usano una virgola (",") per un separatore decimale, è necessario includere la globalizzazione NuGet come descritto in precedenza in questa esercitazione.

Si noti che il form ha utilizzato automaticamente un colore del bordo rosso per evidenziare le caselle di testo che contengono dati non validi ed è stato generato un messaggio di errore di convalida appropriato accanto a ognuna di esse. Gli errori vengono applicati sia sul lato client (utilizzo di JavaScript e jQuery) sia sul lato server (nel caso di un utente con JavaScript disabilitato).

Un vero vantaggio è che non è necessario modificare una singola riga di codice nella classe `MoviesController` o nella vista *create. cshtml* per abilitare questa interfaccia utente di convalida. Il controller e le viste creati in una fase precedente di questa esercitazione hanno selezionato automaticamente le regole di convalida specificate usando gli attributi di convalida delle proprietà della classe `Movie` del modello. Eseguire il test della convalida usando il metodo di azione `Edit` e viene applicata la stessa convalida.

I dati del modulo non vengono inviati al server fino a quando non sono più presenti errori di convalida sul lato client. È possibile verificare questa impostazione inserendo un punto di rottura nel metodo HTTP post, usando lo [strumento Fiddler](http://fiddler2.com/fiddler2/)o gli [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478)di IE.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Come viene eseguita la convalida nel metodo Create View e create Action

Ci si potrebbe chiedere come la convalida dell'interfaccia utente sia stata generata senza aggiornamenti al codice nel controller o nelle viste. L'elenco successivo Mostra i metodi di `Create` nella classe `MovieController` aspetto. Sono rimasti invariati rispetto al modo in cui sono stati creati in precedenza in questa esercitazione.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

Il primo metodo di azione (HTTP GET) `Create` visualizza il modulo di creazione iniziale. La seconda versione (`[HttpPost]`) gestisce l'invio del modulo. Il secondo metodo di `Create` (la versione `HttpPost`) controlla `ModelState.IsValid` per verificare se nel film sono presenti errori di convalida. Il recupero di questa proprietà valuta tutti gli attributi di convalida applicati all'oggetto. Se l'oggetto presenta errori di convalida, il metodo `Create` Visualizza nuovamente il form. Se non sono presenti errori, il metodo salva il nuovo film nel database. Nell'esempio di film, **il modulo non viene inviato al server quando vengono rilevati errori di convalida sul lato client; il secondo** `Create` **metodo non viene mai chiamato**. Se si disabilita JavaScript nel browser, la convalida client viene disabilitata e il metodo HTTP POST `Create` ottiene `ModelState.IsValid` per verificare se nel film sono presenti errori di convalida.

È possibile impostare un punto di interruzione nel metodo `HttpPost Create` e verificare che il metodo non venga mai chiamato, la convalida sul lato client non invierà i dati del modulo in caso di rilevamento di errori di convalida. Se si disabilita JavaScript nel browser, quindi si invia il modulo con errori, verrà raggiunto il punto di interruzione. Si ottiene comunque la convalida completa senza JavaScript. Nell'immagine seguente viene illustrato come disabilitare JavaScript in Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

La figura seguente illustra come disabilitare JavaScript nel browser FireFox.

![](adding-validation/_static/image7.png)

La figura seguente illustra come disabilitare JavaScript nel browser Chrome.

![](adding-validation/_static/image8.png)

Di seguito è riportato il modello di visualizzazione *create. cshtml* che è stato creato in precedenza nell'esercitazione. Viene usata dai metodi di azione illustrati in precedenza per visualizzare il modulo iniziale e per visualizzarlo nuovamente in caso di errore.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Si noti che il codice usa un helper `Html.EditorFor` per restituire l'elemento `<input>` per ogni proprietà `Movie`. Accanto a questo helper è presente una chiamata al metodo helper `Html.ValidationMessageFor`. Questi due metodi helper funzionano con l'oggetto modello passato dal controller alla vista (in questo caso, un oggetto `Movie`). Vengono automaticamente cercati gli attributi di convalida specificati nel modello e i messaggi di errore vengono visualizzati nel modo appropriato.

L'aspetto molto interessante di questo approccio è che né il controller né il modello di vista `Create` sono consapevoli delle regole di convalida effettive applicate o dei messaggi di errore specifici visualizzati. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`. Le stesse regole di convalida vengono applicate automaticamente alla vista `Edit` e ad altri modelli di vista eventualmente creati che modificano il modello.

Se si desidera modificare la logica di convalida in un secondo momento, è possibile eseguire questa operazione in un'unica posizione aggiungendo gli attributi di convalida al modello (in questo esempio, la classe `movie`). Non è necessario preoccuparsi dell'incoerenza delle diverse parti dell'applicazione con la modalità di applicazione delle regole perché tutta la logica di convalida verrà definita in un'unica posizione e usata ovunque. In questo modo il codice rimane molto pulito e facile da gestire e sviluppare. Il che significa che l'utente dovrà rispettare completamente il principio *Dry* .

## <a name="using-datatype-attributes"></a>Utilizzo degli attributi DataType

Aprire il file *Movie.cs* ed esaminare la classe `Movie`. Lo spazio dei nomi [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fornisce attributi di formattazione oltre al set predefinito di attributi di convalida. È già stato applicato un [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valore di enumerazione alla data di rilascio e ai campi relativi ai prezzi. Nel codice seguente vengono illustrate le proprietà `ReleaseDate` e `Price` con l'attributo [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) appropriato.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

Gli attributi [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) forniscono solo gli hint per il motore di visualizzazione per formattare i dati (e forniscono attributi come `<a>` per URL e `<a href="mailto:EmailAddress.com">` per la posta elettronica. È possibile utilizzare l'attributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) per convalidare il formato dei dati. L'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) viene utilizzato per specificare un tipo di dati più specifico del tipo intrinseco del database, ma ***non*** gli attributi di convalida. In questo caso si vuole tenere traccia solo della data e non di data e ora. L' [enumerazione DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornisce per molti tipi di dati, ad esempio *date, Time, PhoneNumber, Currency, EmailAddress* e altro ancora. L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio, è possibile creare un collegamento `mailto:` per [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)ed è possibile specificare un selettore di data per [DataType. date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) nei browser che supportano [HTML5](http://html5.org/). Gli attributi [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) generano attributi di [dati](http://ejohn.org/blog/html-5-data-attributes/) HTML 5 ( *trattini di dati*pronunciati) che possono essere interpretati dai browser HTML 5. Gli attributi [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) non forniscono alcuna convalida.

`DataType.Date` non specifica il formato della data visualizzata. Per impostazione predefinita, il campo dati viene visualizzato in base ai formati predefiniti basati sul valore [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)del server.

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:

[!code-csharp[Main](adding-validation/samples/sample8.cs)]

L'impostazione `ApplyFormatInEditMode` specifica che la formattazione specificata deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica. Per alcuni campi, ad esempio per i valori di valuta, è possibile che non si desideri modificare il simbolo di valuta nella casella di testo.

È possibile usare l'attributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) da solo, ma in genere è consigliabile usare anche l'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) . L'attributo `DataType` fornisce la *semantica* dei dati anziché come eseguirne il rendering in una schermata e offre i seguenti vantaggi che non si ottengono con `DisplayFormat`:

- Il browser può abilitare le funzionalità HTML5, ad esempio per visualizzare un controllo calendario, il simbolo di valuta appropriato per le impostazioni locali, i collegamenti alla posta elettronica e così via.
- Per impostazione predefinita, il browser eseguirà il rendering dei dati usando il formato corretto in base alle [impostazioni locali](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- L'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) può consentire a MVC di scegliere il modello di campo corretto per il rendering dei dati ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) se usato da solo USA il modello di stringa). Per altre informazioni, vedere i [modelli ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)di Brad Wilson. (Anche se scritto per MVC 2, questo articolo è ancora applicabile alla versione corrente di ASP.NET MVC).

Se si usa l'attributo `DataType` con un campo data, è necessario specificare anche l'attributo `DisplayFormat` per assicurarsi che il rendering del campo venga eseguito correttamente nei browser Chrome. Per altre informazioni, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> la convalida jQuery non funziona con l'attributo [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) e [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Ad esempio, il codice seguente visualizzerà sempre un errore di convalida sul lato client, anche quando la data è compreso nell'intervallo specificato:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> È necessario disabilitare la convalida della data jQuery per usare l'attributo di [intervallo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) con [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Non è in genere consigliabile compilare le date rigide nei modelli, quindi l'uso dell'attributo di [intervallo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) e di [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx) è sconsigliato.

Il codice seguente illustra la combinazione di attributi in una sola riga:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=4,6,10,12)]

Nella parte successiva della serie verrà esaminata l'applicazione e verranno apportati alcuni miglioramenti ai metodi `Details` e `Delete` generati automaticamente.

> [!div class="step-by-step"]
> [Precedente](adding-a-new-field.md)
> [Successivo](examining-the-details-and-delete-methods.md)
