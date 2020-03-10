---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: Aggiunta della convalida al modello (C#) | Microsoft Docs
author: Rick-Anderson
description: Creazione di un controller
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 19d86dc0df931a9d135e46209559892b77626cf6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615451"
---
# <a name="adding-validation-to-the-model-c"></a>Aggiunta della convalida al modello (C#)

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

In questa sezione si aggiungerà la logica di convalida al modello di `Movie` e si verificherà che le regole di convalida vengono applicate ogni volta che un utente tenta di creare o modificare un film usando l'applicazione.

## <a name="keeping-things-dry"></a>Conservazione degli elementi

Uno dei principi di base della progettazione di ASP.NET MVC è DRY ("Don't Repeat Yourself"). ASP.NET MVC consiglia di specificare la funzionalità o il comportamento solo una volta e quindi di rifletterla ovunque in un'applicazione. In questo modo si riduce la quantità di codice che è necessario scrivere e si rende molto più semplice la gestione del codice.

Il supporto della convalida fornito da ASP.NET MVC e Entity Framework Code First è un ottimo esempio di principio secco in azione. È possibile specificare in modo dichiarativo le regole di convalida in un'unica posizione (nella classe del modello) e quindi tali regole vengono applicate ovunque nell'applicazione.

Verrà ora esaminato come sfruttare questo supporto per la convalida nell'applicazione Movie.

## <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida al modello di film

Per iniziare, aggiungere una logica di convalida alla classe `Movie`.

Aprire il file *Movie.cs*. Aggiungere un'istruzione `using` all'inizio del file che fa riferimento allo spazio dei nomi [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Lo spazio dei nomi fa parte del .NET Framework. Fornisce un set predefinito di attributi di convalida che è possibile applicare in modo dichiarativo a qualsiasi classe o proprietà.

Aggiornare ora la classe `Movie` per sfruttare i vantaggi degli attributi di convalida [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)e [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) predefiniti. Usare il codice seguente come esempio di come applicare gli attributi.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Gli attributi di convalida specificano il comportamento da applicare per le proprietà del modello a cui vengono applicati. L'attributo `Required` indica che una proprietà deve avere un valore. in questo esempio, un film deve avere valori per le proprietà `Title`, `ReleaseDate`, `Genre`e `Price` per essere valido. L'attributo `Range` vincola un valore all'interno di un intervallo specificato. L'attributo `StringLength` consente di impostare la lunghezza massima di una proprietà stringa e, facoltativamente, la lunghezza minima.

Code First garantisce che le regole di convalida specificate in una classe del modello vengano applicate prima che l'applicazione salvi le modifiche nel database. Il codice seguente, ad esempio, genererà un'eccezione quando viene chiamato il metodo `SaveChanges`, perché sono mancanti diversi valori di proprietà `Movie` richiesti e il prezzo è zero (che non è compreso nell'intervallo valido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

La presenza di regole di convalida applicate automaticamente dall'.NET Framework consente di rendere l'applicazione più affidabile. In questo modo inoltre non è possibile omettere la convalida di un elemento e quindi inserire involontariamente dati errati nel database.

Ecco un listato di codice completo per il file *Movie.cs* aggiornato:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interfaccia utente di errore di convalida in ASP.NET MVC

Eseguire di nuovo l'applicazione e passare all'URL */Movies* .

Fare clic sul collegamento **Crea filmato** per aggiungere un nuovo film. Compilare il modulo con alcuni valori non validi, quindi fare clic sul pulsante **Crea** .

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Si noti che il form ha utilizzato automaticamente un colore di sfondo per evidenziare le caselle di testo che contengono dati non validi ed è stato generato un messaggio di errore di convalida appropriato accanto a ciascuna di esse. I messaggi di errore corrispondono alle stringhe di errore specificate quando è stata annotata la classe `Movie`. Gli errori vengono applicati sia sul lato client (tramite JavaScript) sia sul lato server (nel caso in cui un utente abbia JavaScript disabilitato).

Un vero vantaggio è che non è necessario modificare una singola riga di codice nella classe `MoviesController` o nella vista *create. cshtml* per abilitare questa interfaccia utente di convalida. Il controller e le visualizzazioni creati in precedenza in questa esercitazione hanno selezionato automaticamente le regole di convalida specificate usando gli attributi nella classe del modello `Movie`.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Come viene eseguita la convalida nel metodo Create View e create Action

Ci si potrebbe chiedere come la convalida dell'interfaccia utente sia stata generata senza aggiornamenti al codice nel controller o nelle viste. L'elenco successivo Mostra i metodi di `Create` nella classe `MovieController` aspetto. Sono rimasti invariati rispetto al modo in cui sono stati creati in precedenza in questa esercitazione.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

Il primo metodo di azione Visualizza il modulo di creazione iniziale. Il secondo gestisce il post del form. Il secondo metodo `Create` chiama `ModelState.IsValid` per verificare se nel film sono presenti errori di convalida. La chiamata a questo metodo valuta tutti gli attributi di convalida applicati all'oggetto. Se l'oggetto presenta errori di convalida, il metodo `Create` Visualizza nuovamente il form. Se non sono presenti errori, il metodo salva il nuovo film nel database.

Di seguito è riportato il modello di visualizzazione *create. cshtml* che è stato creato in precedenza nell'esercitazione. Viene usata dai metodi di azione illustrati in precedenza per visualizzare il modulo iniziale e per visualizzarlo nuovamente in caso di errore.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Si noti che il codice usa un helper `Html.EditorFor` per restituire l'elemento `<input>` per ogni proprietà `Movie`. Accanto a questo helper è presente una chiamata al metodo helper `Html.ValidationMessageFor`. Questi due metodi helper funzionano con l'oggetto modello passato dal controller alla vista (in questo caso, un oggetto `Movie`). Vengono automaticamente cercati gli attributi di convalida specificati nel modello e i messaggi di errore vengono visualizzati nel modo appropriato.

L'aspetto più interessante di questo approccio è che né il controller né il modello Create View sono in grado di riconoscere le effettive regole di convalida applicate o i messaggi di errore specifici visualizzati. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`.

Se si desidera modificare la logica di convalida in un secondo momento, è possibile eseguire questa operazione in un'unica posizione. Non è necessario preoccuparsi dell'incoerenza delle diverse parti dell'applicazione con la modalità di applicazione delle regole perché tutta la logica di convalida verrà definita in un'unica posizione e usata ovunque. In questo modo il codice rimane molto pulito e facile da gestire e sviluppare. Il principio DRY sarà ampiamente rispettato.

## <a name="adding-formatting-to-the-movie-model"></a>Aggiunta della formattazione al modello di film

Aprire il file *Movie.cs*. Lo spazio dei nomi [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fornisce attributi di formattazione oltre al set predefinito di attributi di convalida. Si applicheranno l'attributo [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) e un [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valore di enumerazione alla data di rilascio e ai campi Price. Nel codice seguente vengono illustrate le proprietà `ReleaseDate` e `Price` con l'attributo [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) appropriato.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

In alternativa, è possibile impostare in modo esplicito un valore [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) . Il codice seguente illustra la proprietà Data di rilascio con una stringa di formato data (ovvero "d"). Questa operazione viene usata per specificare che non si vuole usare l'ora come parte della data di rilascio.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

Il codice seguente formatta la proprietà `Price` come valuta.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Di seguito è illustrata la classe completa `Movie`.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Eseguire l'applicazione e passare al controller di `Movies`.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Nella parte successiva della serie verrà esaminata l'applicazione e verranno apportati alcuni miglioramenti ai metodi `Details` e `Delete` generati automaticamente.

> [!div class="step-by-step"]
> [Precedente](adding-a-new-field.md)
> [Successivo](improving-the-details-and-delete-methods.md)
