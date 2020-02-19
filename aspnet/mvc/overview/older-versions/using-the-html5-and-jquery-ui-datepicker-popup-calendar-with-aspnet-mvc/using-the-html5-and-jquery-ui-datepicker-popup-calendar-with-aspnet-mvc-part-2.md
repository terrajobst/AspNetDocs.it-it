---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Uso del calendario popup per HTML5 e jQuery UI DatePicker con ASP.NET MVC-parte 2 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il calendario popup DatePicker dell'interfaccia utente di jQuery in un ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 325cc90eb6e717c47863eda6253e0d48d796386b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455893"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Uso del calendario popup per HTML5 e jQuery UI DatePicker con ASP.NET MVC-parte 2

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il calendario popup DatePicker dell'interfaccia utente di jQuery in un'applicazione Web MVC ASP.NET.

## <a name="adding-an-automatic-datetime-template"></a>Aggiunta di un modello DateTime automatico

Nella prima parte di questa esercitazione è stato illustrato come aggiungere attributi al modello per specificare in modo esplicito la formattazione e come è possibile specificare in modo esplicito il modello utilizzato per eseguire il rendering del modello. Ad esempio, l'attributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) nel codice seguente specifica in modo esplicito la formattazione per la proprietà `ReleaseDate`.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

Nell'esempio seguente, l'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , usando l'enumerazione `Date`, specifica che il modello di data deve essere usato per eseguire il rendering del modello. Se nel progetto non è presente alcun modello di data, viene utilizzato il modello di data predefinito.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Tuttavia, ASP. MVC può eseguire la corrispondenza dei tipi usando la convenzione-over-Configuration, cercando un modello che corrisponda al nome di un tipo. In questo modo è possibile creare un modello che formatta automaticamente i dati senza usare alcun attributo o codice. Per questa parte dell'esercitazione verrà creato un modello che viene applicato automaticamente alle proprietà del modello di tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Non è necessario usare un attributo o un'altra configurazione per specificare che il modello deve essere usato per eseguire il rendering di tutte le proprietà del modello di tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Si apprenderà anche come personalizzare la visualizzazione di singole proprietà o anche di singoli campi.

Per iniziare, rimuovere le informazioni di formattazione esistenti e visualizzare le date complete nell'applicazione.

Aprire il file *Movie.cs* e impostare come commento l'attributo `DataType` sulla proprietà `ReleaseDate`:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Premere CTRL+F5 per eseguire l'applicazione.

Si noti che la proprietà `ReleaseDate` ora Visualizza la data e l'ora, perché si tratta dell'impostazione predefinita quando non vengono fornite informazioni di formattazione.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Aggiunta di stili CSS per il test di nuovi modelli

Prima di creare un modello per la formattazione delle date, è necessario aggiungere alcune regole di stile CSS che è possibile applicare ai nuovi modelli. Queste informazioni consentiranno di verificare che la pagina di cui è stato eseguito il rendering stia usando il nuovo modello.

Aprire il file *Content\Site.cs*e aggiungere le seguenti regole CSS alla fine del file:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Aggiunta di modelli di visualizzazione DateTime

A questo punto è possibile creare il nuovo modello. Nella cartella *Views\Movies* creare una cartella *DisplayTemplates* .

Nella cartella *Views\Shared* creare una cartella *DisplayTemplates* e una cartella *EditorTemplates* .

I modelli di visualizzazione nella cartella *Views\Shared\DisplayTemplates.* verranno utilizzati da tutti i controller. I modelli di visualizzazione nella cartella *Views\Movie\DisplayTemplates* verranno utilizzati solo dal controller `Movie`. Se in entrambe le cartelle è presente un modello con lo stesso nome, il modello nella cartella *Views\Movie\DisplayTemplates* , ovvero il modello più specifico, avrà la precedenza sulle visualizzazioni restituite dal controller di `Movie`.

In **Esplora soluzioni**espandere la cartella *views* , espandere la cartella *Shared* , quindi fare clic con il pulsante destro del mouse sulla cartella *Views\Shared\DisplayTemplates.* .

Fare clic su **Aggiungi** e quindi su **Visualizza**. Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** .

Nella casella **nome visualizzazione** Digitare `DateTime`. È necessario usare questo nome per trovare la corrispondenza con il nome del tipo.

Selezionare la casella **di controllo Crea come visualizzazione parziale** . Verificare che le caselle di controllo **Usa un layout o una pagina master** e **Crea una visualizzazione fortemente tipizzata** non siano selezionate.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Fare clic su **Add**. Viene creato un modello *DateTime. cshtml* in *Views\Shared\DisplayTemplates.* .

Nella figura seguente viene illustrata la cartella *views* in **Esplora soluzioni** dopo la creazione dei modelli di visualizzazione e di editor `DateTime`.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Aprire il file *Views\Shared\DisplayTemplates\DateTime.cshtml* e aggiungere il markup seguente, che usa il metodo [String. Format](https://msdn.microsoft.com/library/system.string.format.aspx) per formattare la proprietà come data senza l'ora. Il formato di `{0:d}` specifica un formato di data breve.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Ripetere questo passaggio per creare un modello di `DateTime` nella cartella *Views\Movie\DisplayTemplates* . Usare il codice seguente nel file *Views\Movie\DisplayTemplates\DateTime.cshtml* .

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

La `loud-1` classe CSS causa la visualizzazione della data in grassetto in rosso. È stata aggiunta la `loud-1` classe CSS come misura temporanea, in modo da poter vedere facilmente quando viene usato questo particolare modello.

Ciò che è stato fatto è stato creato e personalizzato i modelli che ASP.NET userà per visualizzare le date. Il modello più generale (nella cartella *Views\Shared\DisplayTemplates.* ) Visualizza una semplice data breve. Il modello specifico per il controller di `Movie` (nella cartella *Views\Movies\DisplayTemplates* ) Visualizza una data breve, anch ' essa formattata come testo in grassetto.

Premere CTRL+F5 per eseguire l'applicazione. Il browser esegue il rendering della visualizzazione dell'indice per l'applicazione.

La proprietà `ReleaseDate` ora Visualizza la data in un tipo di carattere rosso in grassetto senza l'ora. In questo modo è possibile verificare che l'helper basato su modelli `DateTime` nella cartella *Views\Movies\DisplayTemplates* sia selezionato sul supporto basato su modelli `DateTime` nella cartella condivisa (*Views\Shared\DisplayTemplates.* ).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Rinominare ora il file *Views\Movies\DisplayTemplates\DateTime.cshtml* in *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Premere CTRL+F5 per eseguire l'applicazione.

Questa volta la proprietà `ReleaseDate` Visualizza una data senza l'ora e senza il carattere rosso grassetto. Viene illustrato che un modello con il nome del tipo di dati (in questo caso `DateTime`) viene usato automaticamente per visualizzare tutte le proprietà del modello di quel tipo. Dopo aver rinominato il file *DateTime. cshtml* in *LoudDateTime. cshtml*, ASP.NET non è più stato trovato un modello nella cartella *Views\Movies\DisplayTemplates* , quindi è stato usato il modello *datetime. cshtml* dalla cartella * Views\Movies\Shared\*.

Per la corrispondenza dei modelli non viene fatta distinzione tra maiuscole e minuscole, pertanto è possibile che sia stato creato il nome del file modello con qualsiasi maiuscole Ad esempio, *DateTime. cshtml, DateTime. cshtml*e *DateTime. cshtml* corrispondono al tipo `DateTime`.)

Per esaminare: a questo punto, il campo `ReleaseDate` viene visualizzato usando il modello *Views\Movies\DisplayTemplates\DateTime.cshtml* , che Visualizza i dati usando un formato di data breve, ma in caso contrario non aggiunge alcun formato speciale.

### <a name="using-uihint-to-specify-a-display-template"></a>Utilizzo di UIHint per specificare un modello di visualizzazione

Se l'applicazione Web dispone di molti campi `DateTime` e per impostazione predefinita si desidera visualizzarli tutti o la maggior parte del formato di data e ora, il modello *DateTime. cshtml* è un approccio valido. Tuttavia, se si dispone di alcune date in cui si desidera visualizzare la data e l'ora complete? Nessun problema. È possibile creare un modello aggiuntivo e usare l'attributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) per specificare la formattazione per la data e l'ora complete. È quindi possibile applicare il modello in modo selettivo. È possibile utilizzare l'attributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) a livello di modello oppure è possibile specificare il modello all'interno di una visualizzazione. In questa sezione verrà illustrato come utilizzare l'attributo `UIHint` per modificare selettivamente la formattazione per alcune istanze dei campi di data e ora.

Aprire il file *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* e sostituire il codice esistente con il codice seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

In questo modo viene visualizzata la data e l'ora complete e viene aggiunta la classe CSS che rende il testo verde e grande.

Aprire il file *Movie.cs* e aggiungere l'attributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) alla proprietà `ReleaseDate`, come illustrato nell'esempio seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Questo indica a ASP.NET MVC che, quando Visualizza la proprietà `ReleaseDate` (in particolare e non solo gli oggetti `DateTime`), deve usare il modello *LoudDateTime. cshtml* .

Premere CTRL+F5 per eseguire l'applicazione.

Si noti che la proprietà `ReleaseDate` ora Visualizza la data e l'ora in un tipo di carattere verde di grandi dimensioni.

Tornare all'attributo `UIHint` nel file *Movie.cs* e impostarlo come commento, in modo che non venga usato il modello *LoudDateTime. cshtml* . Eseguire di nuovo l'applicazione. La data di rilascio non viene visualizzata in grande e verde. In questo, viene verificato che il modello *Views\Shared\DisplayTemplates\DateTime.cshtml* venga utilizzato nelle visualizzazioni index e Details.

Come indicato in precedenza, è anche possibile applicare un modello in una vista, che consente di applicare il modello a una singola istanza di alcuni dati. Aprire la visualizzazione *Views\Movies\Details.cshtml* . Aggiungere `"LoudDateTime"` come secondo parametro della chiamata [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) per il campo `ReleaseDate`. Ecco il codice completo:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Specifica che il modello di `LoudDateTime` deve essere utilizzato per visualizzare la proprietà del modello, indipendentemente dagli attributi applicati al modello.

Premere CTRL+F5 per eseguire l'applicazione.

Verificare che la pagina di indice del film usi il modello *Views\Shared\DisplayTemplates\DateTime.cshtml* (rosso grassetto) e che la pagina *Movie\Details* usi il modello *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* (grande e verde).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Nella sezione successiva verrà creato un modello per un tipo complesso.

> [!div class="step-by-step"]
> [Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Successivo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
