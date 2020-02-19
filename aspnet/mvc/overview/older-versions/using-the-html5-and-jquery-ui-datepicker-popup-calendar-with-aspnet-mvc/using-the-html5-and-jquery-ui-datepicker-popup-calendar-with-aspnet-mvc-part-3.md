---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Uso del calendario popup per HTML5 e jQuery UI DatePicker con ASP.NET MVC-parte 3 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il calendario popup DatePicker dell'interfaccia utente di jQuery in un ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457895"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Uso del calendario popup per HTML5 e jQuery UI DatePicker con ASP.NET MVC-parte 3

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il calendario popup DatePicker dell'interfaccia utente di jQuery in un'applicazione Web MVC ASP.NET.

## <a name="working-with-complex-types"></a>Utilizzo di tipi complessi

In questa sezione verrà creata una classe Address e verrà illustrato come creare un modello per visualizzarlo.

Nella cartella *Models (modelli* ) creare un nuovo file di classe denominato *Person.cs* , in cui verranno inseriti due tipi: una classe `Person` e una classe `Address`. La classe `Person` conterrà una proprietà tipizzata come `Address`. Il tipo di `Address` è un tipo complesso, ovvero non è uno dei tipi incorporati come `int`, `string`o `double`. Dispone invece di diverse proprietà. Il codice per le nuove classi ha un aspetto simile al seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

Nel controller `Movie` aggiungere la seguente azione `PersonDetail` per visualizzare un'istanza Person:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Aggiungere quindi il codice seguente al controller `Movie` per popolare il modello di `Person` con alcuni dati di esempio:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Aprire il file *Views\Movies\PersonDetail.cshtml* e aggiungere il markup seguente per la visualizzazione `PersonDetail`.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Premere CTRL + F5 per eseguire l'applicazione e passare a *Movies/PersonDetail*.

La vista `PersonDetail` non contiene il tipo complesso `Address`, come si può vedere in questa schermata. Non viene visualizzato alcun indirizzo.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

I dati del modello di `Address` non vengono visualizzati perché si tratta di un tipo complesso. Per visualizzare le informazioni sull'indirizzo, aprire di nuovo il file *Views\Movies\PersonDetail.cshtml* e aggiungere il markup seguente.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Il markup completo per la visualizzazione `PersonDetail` ora è simile al seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Eseguire di nuovo l'applicazione e visualizzare la visualizzazione `PersonDetail`. Vengono ora visualizzate le informazioni sull'indirizzo:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Creazione di un modello per un tipo complesso

In questa sezione verrà creato un modello che verrà usato per eseguire il rendering del tipo complesso `Address`. Quando si crea un modello per il tipo di `Address`, ASP.NET MVC può usarlo automaticamente per formattare un modello di indirizzo in qualsiasi punto dell'applicazione. In questo modo è possibile controllare il rendering del tipo di `Address` da una sola posizione nell'applicazione.

Nella cartella *Views\Shared\DisplayTemplates.* creare una visualizzazione parziale fortemente tipizzata **Indirizzo**denominato:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Fare clic su **Aggiungi**, quindi aprire il nuovo file *Views\Shared\DisplayTemplates\Address.cshtml* . La nuova vista contiene il markup generato seguente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Eseguire l'applicazione e visualizzare la visualizzazione `PersonDetail`. Questa volta, il modello di `Address` appena creato viene usato per visualizzare il tipo complesso `Address`, quindi la visualizzazione è simile alla seguente:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Riepilogo: modi per specificare il formato e il modello di visualizzazione del modello

Si è notato che è possibile specificare il formato o il modello per una proprietà del modello usando gli approcci seguenti:

- Applicazione dell'attributo `DisplayFormat` a una proprietà nel modello. Ad esempio, il codice seguente causa la visualizzazione della data senza l'ora:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Applicare un attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) a una proprietà nel modello e specificare il tipo di dati. Ad esempio, il codice seguente causa la visualizzazione della data senza l'ora.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Se l'applicazione contiene un modello *date. cshtml* nella cartella *Views\Shared\DisplayTemplates.* o nella cartella *Views\Movies\DisplayTemplates* , il modello verrà usato per eseguire il rendering della proprietà `DateTime`. In caso contrario, il sistema di creazione modelli ASP.NET predefinito Visualizza la proprietà come data.
- Creazione di un modello di visualizzazione nella cartella *Views\Shared\DisplayTemplates.* o nella cartella *Views\Movies\DisplayTemplates* il cui nome corrisponda al tipo di dati che si desidera formattare. Si è notato, ad esempio, che *Views\Shared\DisplayTemplates\DateTime.cshtml* è stato utilizzato per eseguire il rendering `DateTime` proprietà in un modello, senza aggiungere un attributo al modello e senza aggiungere markup alle visualizzazioni.
- Utilizzo dell'attributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) sul modello per specificare il modello per la visualizzazione della proprietà del modello.
- Aggiunta esplicita del nome del modello di visualizzazione alla chiamata [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) in una vista.

L'approccio usato dipende da ciò che è necessario eseguire nell'applicazione. Non è insolito combinare questi approcci per ottenere esattamente il tipo di formattazione necessario.

Nella sezione successiva si passerà a un po' e si passerà dalla personalizzazione della modalità di visualizzazione dei dati per personalizzare la modalità di immissione. Per specificare le date, è possibile associare l'oggetto DatePicker di jQuery alle visualizzazioni di modifica nell'applicazione.

> [!div class="step-by-step"]
> [Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Successivo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
