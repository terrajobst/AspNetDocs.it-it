---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Uso della Entity Framework 4,0 e del controllo ObjectDataSource, parte 3: ordinamento e filtro | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal Introduzione con la serie di esercitazioni Entity Framework 4,0. È...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631670"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Uso della Entity Framework 4,0 e del controllo ObjectDataSource, parte 3: ordinamento e filtro

di [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal [Introduzione con la](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni Entity Framework 4,0. Se le esercitazioni precedenti non sono state completate, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) creata. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creata dalla serie di esercitazioni complete. In caso di domande sulle esercitazioni, è possibile pubblicarle nel [Forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).

Nell'esercitazione precedente è stato implementato il modello di repository in un'applicazione Web a più livelli che usa il Entity Framework e il controllo `ObjectDataSource`. In questa esercitazione viene illustrato come eseguire operazioni di ordinamento e filtro e gestire scenari Master-Detail. Verranno aggiunti i miglioramenti seguenti alla pagina *Departments. aspx* :

- Casella di testo che consente agli utenti di selezionare i reparti in base al nome.
- Elenco di corsi per ogni reparto visualizzato nella griglia.
- Possibilità di ordinare facendo clic sulle intestazioni di colonna.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Aggiunta della possibilità di ordinare le colonne GridView

Aprire la pagina *Departments. aspx* e aggiungere un attributo `SortParameterName="sortExpression"` al controllo `ObjectDataSource` denominato `DepartmentsObjectDataSource`. In un secondo momento verrà creato un `GetDepartments` metodo che accetta un parametro denominato `sortExpression`. Il markup per il tag di apertura del controllo è ora simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Aggiungere l'attributo `AllowSorting="true"` al tag di apertura del controllo `GridView`. Il markup per il tag di apertura del controllo è ora simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

In *Departments.aspx.cs*impostare l'ordinamento predefinito chiamando il metodo di `Sort` del controllo `GridView` dal metodo `Page_Load`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

È possibile aggiungere codice che ordina o filtra nella classe della logica di business o nella classe del repository. Se si esegue questa operazione nella classe della logica di business, le operazioni di ordinamento o filtro verranno eseguite dopo che i dati vengono recuperati dal database, perché la classe della logica di business sta utilizzando un oggetto `IEnumerable` restituito dal repository. Se si aggiunge codice di ordinamento e filtro nella classe del repository e si esegue questa operazione prima che un'espressione LINQ o una query di oggetto venga convertita in un oggetto `IEnumerable`, i comandi verranno passati al database per l'elaborazione, che in genere è più efficiente. In questa esercitazione verrà implementato l'ordinamento e il filtro in modo che l'elaborazione venga eseguita dal database, ovvero nel repository.

Per aggiungere la funzionalità di ordinamento, è necessario aggiungere un nuovo metodo all'interfaccia del repository e alle classi del repository, nonché alla classe della logica di business. Nel file *ISchoolRepository.cs* aggiungere un nuovo `GetDepartments` metodo che accetta un parametro di `sortExpression` che verrà usato per ordinare l'elenco dei reparti restituiti:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

Il parametro `sortExpression` specifica la colonna in cui eseguire l'ordinamento e la direzione di ordinamento.

Aggiungere il codice per il nuovo metodo al file *SchoolRepository.cs* :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Modificare il metodo `GetDepartments` senza parametri esistente per chiamare il nuovo metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

Nel progetto di test aggiungere il nuovo metodo seguente a *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Se si intende creare unit test che dipendono da questo metodo che restituisce un elenco ordinato, è necessario ordinare l'elenco prima di restituirlo. In questa esercitazione non verranno creati test, quindi il metodo può solo restituire l'elenco non ordinato dei reparti.

Nel file *SchoolBL.cs* aggiungere il nuovo metodo seguente alla classe della logica di business:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Questo codice passa il parametro sort al metodo del repository.

Eseguire la pagina *repartitions. aspx* .

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

È ora possibile fare clic su qualsiasi intestazione di colonna per ordinare in base a tale colonna. Se la colonna è già ordinata, facendo clic sull'intestazione viene invertita la direzione di ordinamento.

## <a name="adding-a-search-box"></a>Aggiunta di una casella di ricerca

In questa sezione si aggiungerà una casella di testo di ricerca, lo si collegherà al controllo `ObjectDataSource` usando un parametro di controllo e si aggiungerà un metodo alla classe della logica di business per supportare l'applicazione di filtri.

Aprire la pagina *Departments. aspx* e aggiungere il markup seguente tra l'intestazione e il primo controllo `ObjectDataSource`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

Nel controllo `ObjectDataSource` denominato `DepartmentsObjectDataSource`eseguire le operazioni seguenti:

- Aggiungere un elemento `SelectParameters` per un parametro denominato `nameSearchString` che ottiene il valore immesso nel controllo `SearchTextBox`.
- Modificare il valore dell'attributo `SelectMethod` in `GetDepartmentsByName`. Questo metodo verrà creato in un secondo momento.

Il markup per il controllo `ObjectDataSource` ora è simile all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

In *ISchoolRepository.cs*aggiungere un `GetDepartmentsByName` metodo che accetta i parametri `sortExpression` e `nameSearchString`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

In *SchoolRepository.cs*aggiungere il nuovo metodo seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Questo codice usa un metodo di `Where` per selezionare gli elementi che contengono la stringa di ricerca. Se la stringa di ricerca è vuota, verranno selezionati tutti i record. Si noti che quando si specificano le chiamate al metodo in un'unica istruzione come questa (`Include`, quindi `OrderBy`, quindi `Where`), il metodo `Where` deve essere sempre l'ultimo.

Modificare il metodo `GetDepartments` esistente che accetta un parametro di `sortExpression` per chiamare il nuovo metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

In *MockSchoolRepository.cs* nel progetto di test aggiungere il nuovo metodo seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

In *SchoolBL.cs*aggiungere il nuovo metodo seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Eseguire la pagina *repartitions. aspx* e immettere una stringa di ricerca per assicurarsi che la logica di selezione funzioni. Lasciare vuota la casella di testo e provare a eseguire una ricerca per assicurarsi che vengano restituiti tutti i record.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Aggiunta di una colonna dettagli per ogni riga della griglia

Successivamente, si desidera visualizzare tutti i corsi per ogni reparto visualizzato nella cella a destra della griglia. A tale scopo, si utilizzerà un controllo `GridView` annidato e lo si aggiungerà ai dati della proprietà di navigazione `Courses` dell'entità `Department`.

Aprire *repartitions. aspx* e nel markup per il controllo `GridView` specificare un gestore per l'evento `RowDataBound`. Il markup per il tag di apertura del controllo è ora simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Aggiungere un nuovo elemento `TemplateField` dopo il campo modello `Administrator`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Questo markup crea un controllo `GridView` annidato che mostra il numero di corso e il titolo di un elenco di corsi. Non specifica un'origine dati perché verrà eseguita la relativa associazione nel codice del gestore `RowDataBound`.

Aprire *Departments.aspx.cs* e aggiungere il gestore seguente per l'evento `RowDataBound`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Questo codice ottiene l'entità `Department` dagli argomenti dell'evento, converte la proprietà di navigazione `Courses` in una raccolta di `List` e DataBinding il `GridView` annidato alla raccolta.

Aprire il file *SchoolRepository.cs* e specificare il caricamento eager per la proprietà di navigazione `Courses` chiamando il metodo `Include` nella query di oggetto creata nel metodo `GetDepartmentsByName`. L'istruzione `return` nel metodo `GetDepartmentsByName` ora è simile all'esempio seguente.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Eseguire la pagina. Oltre alla funzionalità di ordinamento e filtro aggiunta in precedenza, il controllo GridView ora Mostra i dettagli dei corsi annidati per ogni reparto.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Questa operazione completa l'introduzione a scenari di ordinamento, filtro e dettagli master. Nell'esercitazione successiva si vedrà come gestire la concorrenza.

> [!div class="step-by-step"]
> [Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Successivo](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
