---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Aggiunta della convalida al modello | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: c9f6699c5d3500d4c1fcade9252aeb9dd92983da
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455958"
---
# <a name="adding-validation-to-the-model"></a>Aggiunta della convalida al modello

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.

In questa sezione si aggiungerà la logica di convalida al modello di `Movie` e si verificherà che le regole di convalida vengono applicate ogni volta che un utente tenta di creare o modificare un film usando l'applicazione.

## <a name="keeping-things-dry"></a>Conservazione degli elementi

Uno dei principi di base della progettazione di ASP.NET MVC è DRY (&quot;Don't Repeat Yourself&quot;). ASP.NET MVC consiglia di specificare la funzionalità o il comportamento solo una volta e quindi di rifletterla ovunque in un'applicazione. In questo modo si riduce la quantità di codice che è necessario scrivere e il codice che si scrive meno soggetto a errori e si semplifica la manutenzione.

Il supporto della convalida fornito da ASP.NET MVC e Entity Framework Code First è un ottimo esempio di principio secco in azione. È possibile specificare in modo dichiarativo le regole di convalida in un'unica posizione (nella classe del modello) e le regole vengono applicate ovunque nell'applicazione.

Verrà ora esaminato come sfruttare questo supporto per la convalida nell'applicazione Movie.

## <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida al modello di film

Per iniziare, aggiungere una logica di convalida alla classe `Movie`.

Aprire il file *Movie.cs*. Aggiungere un'istruzione `using` all'inizio del file che fa riferimento allo spazio dei nomi [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Si noti che lo spazio dei nomi non contiene `System.Web`. DataAnnotations fornisce un set predefinito di attributi di convalida che è possibile applicare in modo dichiarativo a qualsiasi classe o proprietà.

Aggiornare ora la classe `Movie` per sfruttare i vantaggi degli attributi di convalida [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)e [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) predefiniti. Usare il codice seguente come esempio di come applicare gli attributi.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Eseguire l'applicazione e verrà nuovamente ottenuto il seguente errore di run-time:

***Il modello che supporta il contesto ' MovieDBContext ' è stato modificato dopo la creazione del database. Si consiglia di utilizzare Migrazioni Code First per aggiornare il database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Per aggiornare lo schema, si utilizzeranno le migrazioni. Compilare la soluzione, quindi aprire la finestra **console di gestione pacchetti** e immettere i comandi seguenti:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova classe derivata `DbMigration` con il nome specificato (*AddDataAnnotationsMig*) e nel metodo `Up` è possibile visualizzare il codice che aggiorna i vincoli dello schema. I campi `Title` e `Genre` non sono più Nullable (ovvero è necessario immettere un valore) e il campo `Rating` ha una lunghezza massima di 5.

Gli attributi di convalida specificano il comportamento da applicare per le proprietà del modello a cui vengono applicati. L'attributo `Required` indica che una proprietà deve avere un valore. in questo esempio, un film deve avere valori per le proprietà `Title`, `ReleaseDate`, `Genre`e `Price` per essere valido. L'attributo `Range` vincola un valore all'interno di un intervallo specificato. L'attributo `StringLength` consente di impostare la lunghezza massima di una proprietà stringa e, facoltativamente, la lunghezza minima. I tipi intrinseci (ad esempio `decimal, int, float, DateTime`) sono necessari per impostazione predefinita e non richiedono l'attributo `Required`.

Code First garantisce che le regole di convalida specificate in una classe del modello vengano applicate prima che l'applicazione salvi le modifiche nel database. Il codice seguente, ad esempio, genererà un'eccezione quando viene chiamato il metodo `SaveChanges`, perché sono mancanti diversi valori di proprietà `Movie` richiesti e il prezzo è zero (che non è compreso nell'intervallo valido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

La presenza di regole di convalida applicate automaticamente dall'.NET Framework consente di rendere l'applicazione più affidabile. In questo modo inoltre non è possibile omettere la convalida di un elemento e quindi inserire involontariamente dati errati nel database.

Ecco un listato di codice completo per il file *Movie.cs* aggiornato:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interfaccia utente di errore di convalida in ASP.NET MVC

Eseguire di nuovo l'applicazione e passare all'URL */Movies* .

Fare clic sul collegamento **Crea nuovo** per aggiungere un nuovo film. Compilare il modulo con alcuni valori non validi, quindi fare clic sul pulsante **Crea** .

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> per supportare la convalida jQuery per le impostazioni locali diverse dall'inglese che usano una virgola (&quot;,&quot;) per un separatore decimale, è necessario includere *globalizzate. js* e il file *Cultures/globalizzate. culture. js* specifico (da [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript per usare `Globalize.parseFloat`. Nel codice seguente vengono illustrate le modifiche apportate al file Views\Movies\Edit.cshtml per l'utilizzo con &quot;le impostazioni cultura&quot; fr-FR:

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Si noti che il form ha utilizzato automaticamente un colore del bordo rosso per evidenziare le caselle di testo che contengono dati non validi ed è stato generato un messaggio di errore di convalida appropriato accanto a ognuna di esse. Gli errori vengono applicati sia sul lato client (utilizzo di JavaScript e jQuery) sia sul lato server (nel caso di un utente con JavaScript disabilitato).

Un vero vantaggio è che non è necessario modificare una singola riga di codice nella classe `MoviesController` o nella vista *create. cshtml* per abilitare questa interfaccia utente di convalida. Il controller e le viste creati in una fase precedente di questa esercitazione hanno selezionato automaticamente le regole di convalida specificate usando gli attributi di convalida delle proprietà della classe `Movie` del modello.

Si potrebbe notare che le proprietà `Title` e `Genre`, l'attributo obbligatorio non viene applicato fino a quando non si invia il modulo (premere il pulsante **Crea** ) oppure immettere il testo nel campo di input e rimuoverlo. Per un campo inizialmente vuoto, ad esempio i campi nella visualizzazione create, che include solo l'attributo required e nessun altro attributo di convalida, è possibile eseguire le operazioni seguenti per attivare la convalida:

1. Tab nel campo.
2. Immettere il testo.
3. Uscire dalla scheda tramite tabulazione.
4. Tornare al campo.
5. Rimuovere il testo.
6. Uscire dalla scheda tramite tabulazione.

La sequenza precedente attiverà la convalida richiesta senza premere il pulsante Submit (Invia). Quando si preme il pulsante Invia senza immettere nessuno dei campi, viene attivata la convalida lato client. I dati del modulo non vengono inviati al server fino a quando non sono più presenti errori di convalida sul lato client. È possibile testare questa impostazione inserendo un punto di rottura nel metodo HTTP post o usando lo [strumento Fiddler](http://fiddler2.com/fiddler2/) o gli strumenti di [sviluppo](https://msdn.microsoft.com/ie/aa740478)di IE 9 F12.

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Come viene eseguita la convalida nel metodo Create View e create Action

Ci si potrebbe chiedere come la convalida dell'interfaccia utente sia stata generata senza aggiornamenti al codice nel controller o nelle viste. L'elenco successivo Mostra i metodi di `Create` nella classe `MovieController` aspetto. Sono rimasti invariati rispetto al modo in cui sono stati creati in precedenza in questa esercitazione.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

Il primo metodo di azione (HTTP GET) `Create` visualizza il modulo di creazione iniziale. La seconda versione (`[HttpPost]`) gestisce l'invio del modulo. Il secondo metodo `Create` (la versione `HttpPost`) chiama `ModelState.IsValid` per verificare se esistono errori di convalida per il film. La chiamata a questo metodo valuta tutti gli attributi di convalida applicati all'oggetto. Se l'oggetto presenta errori di convalida, il metodo `Create` visualizza di nuovo il modulo. Se non sono presenti errori, il metodo salva il nuovo film nel database. Nell'esempio di film usato, **il modulo non viene inviato al server quando vengono rilevati errori di convalida sul lato client; il secondo** metodo `Create`non **viene mai chiamato**. Se si disabilita JavaScript nel browser, la convalida client viene disabilitata e il metodo HTTP POST `Create` chiama `ModelState.IsValid` per verificare se nel film sono presenti errori di convalida.

È possibile impostare un punto di interruzione nel metodo `HttpPost Create` e verificare che il metodo non venga mai chiamato, la convalida sul lato client non invierà i dati del modulo in caso di rilevamento di errori di convalida. Se si disabilita JavaScript nel browser, quindi si invia il modulo con errori, verrà raggiunto il punto di interruzione. Si ottiene comunque la convalida completa senza JavaScript. Nell'immagine seguente viene illustrato come disabilitare JavaScript in Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

La figura seguente illustra come disabilitare JavaScript nel browser FireFox.

![](adding-validation-to-the-model/_static/image5.png)

La figura seguente illustra come disabilitare JavaScript con il browser Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Di seguito è riportato il modello di visualizzazione *create. cshtml* che è stato creato in precedenza nell'esercitazione. Viene usata dai metodi di azione illustrati in precedenza per visualizzare il modulo iniziale e per visualizzarlo nuovamente in caso di errore.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Si noti che il codice usa un helper `Html.EditorFor` per restituire l'elemento `<input>` per ogni proprietà `Movie`. Accanto a questo helper è presente una chiamata al metodo helper `Html.ValidationMessageFor`. Questi due metodi helper funzionano con l'oggetto modello passato dal controller alla vista (in questo caso, un oggetto `Movie`). Vengono automaticamente cercati gli attributi di convalida specificati nel modello e i messaggi di errore vengono visualizzati nel modo appropriato.

L'aspetto più interessante di questo approccio è che né il controller né il modello Create View sono in grado di riconoscere le effettive regole di convalida applicate o i messaggi di errore specifici visualizzati. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`. Queste stesse regole di convalida vengono applicate automaticamente alla visualizzazione di modifica e a tutti gli altri modelli di viste che è possibile creare per modificare il modello.

Se si desidera modificare la logica di convalida in un secondo momento, è possibile eseguire questa operazione in un'unica posizione aggiungendo gli attributi di convalida al modello (in questo esempio, la classe `movie`). Non è necessario preoccuparsi dell'incoerenza delle diverse parti dell'applicazione con la modalità di applicazione delle regole perché tutta la logica di convalida verrà definita in un'unica posizione e usata ovunque. In questo modo il codice rimane molto pulito e facile da gestire e sviluppare. Il principio DRY sarà ampiamente rispettato.

## <a name="adding-formatting-to-the-movie-model"></a>Aggiunta della formattazione al modello di film

Aprire il file *Movie.cs* ed esaminare la classe `Movie`. Lo spazio dei nomi [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fornisce attributi di formattazione oltre al set predefinito di attributi di convalida. È già stato applicato un [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valore di enumerazione alla data di rilascio e ai campi relativi ai prezzi. Nel codice seguente vengono illustrate le proprietà `ReleaseDate` e `Price` con l'attributo [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) appropriato.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Gli attributi [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) non sono attributi di convalida, vengono usati per indicare al motore di visualizzazione come eseguire il rendering del codice HTML. Nell'esempio precedente, l'attributo `DataType.Date` Visualizza le date dei film solo come date, senza tempo. Gli attributi [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) seguenti, ad esempio, non convalidano il formato dei dati:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Gli attributi elencati sopra forniscono solo gli hint per il motore di visualizzazione per formattare i dati (e forniscono attributi come &lt;un&gt; per URL e &lt;un href =&quot;mailto:EmailAddress. com&quot;&gt; per la posta elettronica. È possibile utilizzare l'attributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) per convalidare il formato dei dati.

Un approccio alternativo all'uso degli attributi di `DataType`, è possibile impostare in modo esplicito un valore [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) . Nel codice seguente viene illustrata la proprietà Data di rilascio con una stringa di formato data, ovvero &quot;d&quot;. Questa operazione viene usata per specificare che non si vuole usare l'ora come parte della data di rilascio.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Di seguito è illustrata la classe completa `Movie`.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Eseguire l'applicazione e passare al controller di `Movies`. La data di rilascio e il prezzo sono formattati correttamente. L'immagine seguente mostra la data di rilascio e il prezzo usando &quot;&quot; fr-FR come impostazioni cultura.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

L'immagine seguente Mostra gli stessi dati visualizzati con le impostazioni cultura predefinite (Inglese USA).

![](adding-validation-to-the-model/_static/image8.png)

Nella parte successiva della serie verrà esaminata l'applicazione e verranno apportati alcuni miglioramenti ai metodi `Details` e `Delete` generati automaticamente.

> [!div class="step-by-step"]
> [Precedente](adding-a-new-field-to-the-movie-model-and-table.md)
> [Successivo](examining-the-details-and-delete-methods.md)
