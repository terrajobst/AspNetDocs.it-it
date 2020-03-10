---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Uso del Entity Framework 4,0 e del controllo ObjectDataSource, parte 2: aggiunta di un livello di logica di business e unit test | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal Introduzione con la serie di esercitazioni Entity Framework 4,0. È...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 24344cc33d7c26d7c408db26c0530ef2c708a7d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546767"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Uso della Entity Framework 4,0 e del controllo ObjectDataSource, parte 2: aggiunta di un livello di logica di business e unit test

di [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione Web Contoso University creata dal [Introduzione con la](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni Entity Framework 4,0. Se le esercitazioni precedenti non sono state completate, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) creata. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creata dalla serie di esercitazioni complete. In caso di domande sulle esercitazioni, è possibile pubblicarle nel [Forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).

Nell'esercitazione precedente è stata creata un'applicazione Web a più livelli usando il Entity Framework e il controllo `ObjectDataSource`. In questa esercitazione viene illustrato come aggiungere la logica di business mantenendo il livello di logica di business (BLL) e il livello di accesso ai dati (DAL) separatamente e viene illustrato come creare unit test automatizzati per il BLL.

In questa esercitazione verranno completate le attività seguenti:

- Creare un'interfaccia del repository che dichiara i metodi di accesso ai dati necessari.
- Implementare l'interfaccia del repository nella classe del repository.
- Creare una classe di logica di business che chiama la classe repository per eseguire funzioni di accesso ai dati.
- Connettere il controllo `ObjectDataSource` alla classe della logica di business anziché alla classe del repository.
- Creare un progetto di unit test e una classe di repository che usa raccolte in memoria per l'archivio dati.
- Creare una unit test per la logica di business che si desidera aggiungere alla classe di logica di business, quindi eseguire il test e verificarne l'esito negativo.
- Implementare la logica di business nella classe di logica di business, quindi eseguire di nuovo il unit test e verificarne il passaggio.

Verranno utilizzate le pagine *Departments. aspx* e *DepartmentsAdd. aspx* create nell'esercitazione precedente.

## <a name="creating-a-repository-interface"></a>Creazione di un'interfaccia del repository

Si inizierà creando l'interfaccia del repository.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

Nella cartella *dal* creare un nuovo file di classe, denominarlo *ISchoolRepository.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

L'interfaccia definisce un metodo per ognuno dei metodi CRUD (create, Read, Update, Delete) creati nella classe del repository.

Nella classe `SchoolRepository` in *SchoolRepository.cs*indicare che questa classe implementa l'interfaccia `ISchoolRepository`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Creazione di una classe di logica di business

Successivamente, verrà creata la classe di logica di business. Questa operazione viene eseguita in modo che sia possibile aggiungere la logica di business che verrà eseguita dal controllo `ObjectDataSource`, anche se questa operazione non è ancora presente. Per il momento, la nuova classe di logica di business eseguirà solo le stesse operazioni CRUD eseguite dal repository.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Creare una nuova cartella e denominarla *BLL*. In un'applicazione reale, il livello di logica di business viene in genere implementato come libreria di classi, un progetto separato, ma per mantenere semplice questa esercitazione, le classi BLL verranno mantenute in una cartella di progetto.

Nella cartella *BLL* creare un nuovo file di classe, denominarlo *SchoolBL.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Questo codice crea gli stessi metodi CRUD precedentemente visualizzati nella classe del repository, ma invece di accedere ai metodi di Entity Framework direttamente, chiama i metodi della classe di repository.

La variabile di classe che contiene un riferimento alla classe del repository è definita come un tipo di interfaccia e il codice che crea un'istanza della classe di repository è contenuto in due costruttori. Il costruttore senza parametri verrà utilizzato dal controllo `ObjectDataSource`. Viene creata un'istanza della classe `SchoolRepository` creata in precedenza. L'altro costruttore consente a qualsiasi codice che crea un'istanza della classe di logica di business di passare qualsiasi oggetto che implementi l'interfaccia del repository.

I metodi CRUD che chiamano la classe repository e i due costruttori consentono di usare la classe della logica di business con qualsiasi archivio dati back-end scelto. Non è necessario che la classe della logica di business sia a conoscenza del modo in cui la classe che chiama rende persistenti i dati. Questa operazione viene spesso definita *ignoranza di persistenza*. Ciò facilita il testing unità, perché è possibile connettere la classe della logica di business a un'implementazione del repository che usa un elemento semplice come le raccolte di `List` in memoria per archiviare i dati.

> [!NOTE]
> Tecnicamente, gli oggetti entità non sono ancora in grado di ignorare la persistenza, perché ne viene creata un'istanza dalle classi che ereditano dalla classe `EntityObject` del Entity Framework. Per un'ignoranza di persistenza completa, è possibile utilizzare *oggetti CLR obsoleti*o oggetti *poco*al posto di oggetti che ereditano dalla classe `EntityObject`. L'uso di POCOs esula dall'ambito di questa esercitazione. Per ulteriori informazioni, vedere [testabilità e Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx) sul sito Web MSDN.

A questo punto è possibile connettere i controlli `ObjectDataSource` alla classe della logica di business anziché al repository e verificare che tutto funzioni come prima.

In *repartitions. aspx* e *DepartmentsAdd. aspx*modificare ogni occorrenza di `TypeName="ContosoUniversity.DAL.SchoolRepository"` in `TypeName="ContosoUniversity.BLL.SchoolBL`". Sono disponibili quattro istanze.

Eseguire le pagine *Departments. aspx* e *DepartmentsAdd. aspx* per verificare che continuino a funzionare come in precedenza.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Creazione di un progetto di unit test e di un'implementazione del repository

Aggiungere un nuovo progetto alla soluzione usando il modello di **progetto di test** e denominarlo `ContosoUniversity.Tests`.

Nel progetto di test aggiungere un riferimento a `System.Data.Entity` e aggiungere un riferimento al progetto `ContosoUniversity`.

È ora possibile creare la classe di repository da usare con gli unit test. L'archivio dati per questo repository sarà all'interno della classe.

[![IMAGE12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

Nel progetto di test creare un nuovo file di classe, denominarlo *MockSchoolRepository.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Questa classe di repository ha gli stessi metodi CRUD di quelli che accedono direttamente al Entity Framework, ma funzionano con `List` raccolte in memoria invece che con un database. Questo rende più semplice per una classe di test configurare e convalidare gli unit test per la classe di logica di business.

## <a name="creating-unit-tests"></a>Creazione di unit test

Il modello di progetto di **test** ha creato una classe di unit test stub e l'attività successiva consiste nel modificare questa classe aggiungendo unit test metodi per la logica di business che si desidera aggiungere alla classe della logica di business.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Presso Contoso University ogni singolo istruttore può essere solo l'amministratore di un singolo reparto ed è necessario aggiungere la logica di business per applicare questa regola. Si inizierà aggiungendo i test ed eseguendo i test per verificarne l'esito negativo. Si aggiungerà quindi il codice e si eseguirà di nuovo i test, in modo che vengano visualizzati come superati.

Aprire il file *UnitTest1.cs* e aggiungere `using` istruzioni per la logica di business e i livelli di accesso ai dati creati nel progetto ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Sostituire il metodo `TestMethod1` con i metodi seguenti:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

Il metodo `CreateSchoolBL` crea un'istanza della classe di repository creata per il progetto unit test, che viene quindi passata a una nuova istanza della classe della logica di business. Il metodo usa quindi la classe della logica di business per inserire tre reparti che è possibile usare nei metodi di test.

I metodi di test verificano che la classe della logica di business generi un'eccezione se un utente tenta di inserire un nuovo reparto con lo stesso amministratore di un reparto esistente o se un utente tenta di aggiornare l'amministratore di un reparto impostandolo sull'ID di una persona Chi è già amministratore di un altro reparto.

Non è stata ancora creata la classe Exception, quindi questo codice non verrà compilato. Per compilarlo, fare clic con il pulsante destro del mouse su `DuplicateAdministratorException` e selezionare **genera**, quindi **classe**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

In questo modo viene creata una classe nel progetto di test che può essere eliminata dopo la creazione della classe Exception nel progetto principale. e hanno implementato la logica di business.

Eseguire il progetto di test. Come previsto, i test hanno esito negativo.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Aggiunta della logica di business per eseguire un test superato

Successivamente, verrà implementata la logica di business che rende impossibile impostare come amministratore di un reparto un utente che è già amministratore di un altro reparto. Verrà generata un'eccezione dal livello di logica di business, che verrà quindi intercettata nel livello di presentazione se un utente modifica un reparto e fa clic su **Aggiorna** dopo aver selezionato un utente che è già un amministratore. È anche possibile rimuovere gli istruttori dall'elenco a discesa che sono già amministratori prima di eseguire il rendering della pagina, ma lo scopo è quello di usare il livello di logica di business.

Iniziare creando la classe di eccezione che verrà generata quando un utente tenta di rendere un insegnante l'amministratore di più di un reparto. Nel progetto principale creare un nuovo file di classe nella cartella *BLL* , denominarlo *DuplicateAdministratorException.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

A questo punto, eliminare il file *DuplicateAdministratorException.cs* temporaneo creato nel progetto di test in precedenza per poterlo compilare.

Nel progetto principale aprire il file *SchoolBL.cs* e aggiungere il metodo seguente che contiene la logica di convalida. Il codice fa riferimento a un metodo che verrà creato in un secondo momento.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Questo metodo verrà chiamato quando si inseriscono o si aggiornano `Department` entità per verificare se un altro reparto ha già lo stesso amministratore.

Il codice chiama un metodo per eseguire una ricerca nel database di un'entità `Department` con lo stesso valore `Administrator` proprietà dell'entità da inserire o aggiornare. Se ne viene trovato uno, il codice genera un'eccezione. Non è necessario alcun controllo di convalida se l'entità inserita o aggiornata non ha valore `Administrator` e non viene generata alcuna eccezione se il metodo viene chiamato durante un aggiornamento e l'entità `Department` trovata corrisponde all'entità `Department` da aggiornare.

Chiamare il nuovo metodo dai metodi `Insert` e `Update`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

In *ISchoolRepository.cs*aggiungere la dichiarazione seguente per il nuovo metodo di accesso ai dati:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

In *SchoolRepository.cs*aggiungere l'istruzione `using` seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

In *SchoolRepository.cs*aggiungere il nuovo metodo di accesso ai dati seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Questo codice recupera `Department` entità che hanno un amministratore specificato. È necessario trovare solo un reparto. Tuttavia, poiché nel database non è incorporato alcun vincolo, il tipo restituito è una raccolta nel caso in cui vengano trovati più reparti.

Per impostazione predefinita, quando il contesto dell'oggetto recupera le entità dal database, ne tiene traccia nel gestore dello stato dell'oggetto. Il parametro `MergeOption.NoTracking` specifica che questa verifica non verrà eseguita per la query. Questa operazione è necessaria perché la query potrebbe restituire l'entità esatta che si sta tentando di aggiornare, quindi non sarà possibile alporre tale entità. Se ad esempio si modifica il reparto cronologia nella pagina *Departments. aspx* e si lascia invariato l'amministratore, la query restituirà il reparto cronologia. Se `NoTracking` non è impostato, il contesto dell'oggetto avrebbe già l'entità Department del reparto della cronologia nel relativo gestore dello stato dell'oggetto. Quindi, quando si connette l'entità Department del reparto che viene ricreata dallo stato di visualizzazione, il contesto dell'oggetto genera un'eccezione che afferma `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

In alternativa alla specifica di `MergeOption.NoTracking`, è possibile creare un nuovo contesto dell'oggetto solo per questa query. Poiché il nuovo contesto dell'oggetto avrebbe il proprio gestore di stato dell'oggetto, non vi sarebbero conflitti quando si chiama il metodo `Attach`. Il nuovo contesto dell'oggetto condivide i metadati e la connessione di database con il contesto dell'oggetto originale, quindi la riduzione delle prestazioni di questo approccio alternativo potrebbe essere minima. Questo approccio, tuttavia, introduce l'opzione `NoTracking`, che risulta utile in altri contesti. L'opzione `NoTracking` viene illustrata più avanti in un'esercitazione successiva di questa serie.

Nel progetto di test aggiungere il nuovo metodo di accesso ai dati a *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Questo codice USA LINQ per eseguire la stessa selezione di dati usata dal repository del progetto `ContosoUniversity` LINQ to Entities per.

Eseguire di nuovo il progetto di test. Questa volta il test ha esito positivo.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Gestione delle eccezioni ObjectDataSource

Nel progetto `ContosoUniversity` eseguire la pagina *repartitions. aspx* e provare a modificare l'amministratore per un reparto per un utente che è già amministratore di un altro reparto. Tenere presente che è possibile modificare solo i reparti aggiunti durante questa esercitazione, perché il database è precaricato con dati non validi. Si ottiene la seguente pagina di errore del server:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Non si vuole che gli utenti visualizzino questo tipo di pagina di errore, quindi è necessario aggiungere il codice di gestione degli errori. Aprire *repartitions. aspx* e specificare un gestore per l'evento `OnUpdated` della `DepartmentsObjectDataSource`. Il tag di apertura `ObjectDataSource` ora è simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

In *Departments.aspx.cs*aggiungere l'istruzione `using` seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Aggiungere il seguente gestore per l'evento `Updated`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Se il controllo `ObjectDataSource` rileva un'eccezione quando tenta di eseguire l'aggiornamento, passa l'eccezione nell'argomento dell'evento (`e`) a questo gestore. Il codice nel gestore verifica se l'eccezione è l'eccezione dell'amministratore duplicato. In caso contrario, il codice crea un controllo validator che contiene un messaggio di errore per la visualizzazione del controllo `ValidationSummary`.

Eseguire la pagina e tentare di ricreare l'amministratore di due reparti. Questa volta il controllo `ValidationSummary` Visualizza un messaggio di errore.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Apportare modifiche simili alla pagina *DepartmentsAdd. aspx* . In *DepartmentsAdd. aspx*specificare un gestore per l'evento `OnInserted` della `DepartmentsObjectDataSource`. Il markup risultante sarà simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

In *DepartmentsAdd.aspx.cs*aggiungere la stessa istruzione `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Aggiungere il gestore eventi seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

È ora possibile testare la pagina *DepartmentsAdd.aspx.cs* per verificare che venga gestito correttamente anche il tentativo di rendere un utente amministratore di più di un reparto.

Questa operazione completa l'introduzione all'implementazione del modello di repository per l'uso del controllo `ObjectDataSource` con il Entity Framework. Per ulteriori informazioni sul modello di repository e sulla testabilità, vedere la pagina relativa alla [testabilità e Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx)del white paper MSDN.

Nell'esercitazione seguente verrà illustrato come aggiungere funzionalità di ordinamento e filtro all'applicazione.

> [!div class="step-by-step"]
> [Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Successivo](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
