---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Creazione di un modello di dati più complesso per un'applicazione MVC ASP.NET (4 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9c19ec47ad98a319ddee17db014c4b15e3734778
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595353"
---
# <a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Creazione di un modello di dati più complesso per un'applicazione MVC ASP.NET (4 di 10)

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si riscontra un problema che non è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. È in genere possibile trovare la soluzione al problema confrontando il codice con il codice completato. Per alcuni errori comuni e per informazioni su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Nelle esercitazioni precedenti è stato usato un modello di dati semplice costituito da tre entità. In questa esercitazione verranno aggiunte altre entità e relazioni e sarà possibile personalizzare il modello di dati specificando le regole di formattazione, convalida e mapping del database. Verranno visualizzati due modi per personalizzare il modello di dati: aggiungendo attributi alle classi di entità e aggiungendo codice alla classe del contesto di database.

Al termine dell'operazione le classi di entità verranno incluse nel modello di dati completato, illustrato nella figura seguente:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizzare il modello di dati usando gli attributi

In questa sezione si apprenderà come personalizzare il modello di dati usando attributi che specificano regole di formattazione, convalida e mapping del database. Quindi, in molte delle sezioni seguenti si creerà il modello di dati completo `School` aggiungendo attributi alle classi già create e creando nuove classi per i tipi di entità rimanenti nel modello.

### <a name="the-datatype-attribute"></a>Attributo DataType

Per le date di iscrizione degli studenti, tutte le pagine Web attualmente visualizzano l'ora oltre alla data, anche se l'unico elemento rilevante di questo campo è la data. Mediante gli attributi di annotazione dei dati è possibile modificare il codice per correggere il formato di visualizzazione in tutte le visualizzazioni che visualizzano i dati. Per un esempio di come eseguire questa operazione si aggiunge un attributo alla proprietà `EnrollmentDate` nella classe `Student`.

In *Models\Student.cs*aggiungere un'istruzione `using` per lo spazio dei nomi `System.ComponentModel.DataAnnotations` e aggiungere `DataType` e `DisplayFormat` attributi alla proprietà `EnrollmentDate`, come illustrato nell'esempio seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

L'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) viene utilizzato per specificare un tipo di dati più specifico del tipo intrinseco del database. In questo caso si vuole tenere traccia solo della data e non di data e ora. L' [enumerazione DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornisce per molti tipi di dati, ad esempio *date, Time, PhoneNumber, Currency, EmailAddress* e altro ancora. L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio, è possibile creare un collegamento `mailto:` per [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)ed è possibile specificare un selettore di data per [DataType. date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) nei browser che supportano [HTML5](http://html5.org/). Gli attributi [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) generano attributi di [dati](http://ejohn.org/blog/html-5-data-attributes/) HTML 5 ( *trattini di dati*pronunciati) che possono essere interpretati dai browser HTML 5. Gli attributi [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) non forniscono alcuna convalida.

`DataType.Date` non specifica il formato della data visualizzata. Per impostazione predefinita, il campo dati viene visualizzato in base ai formati predefiniti basati sul valore [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)del server.

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

L'impostazione `ApplyFormatInEditMode` specifica che la formattazione specificata deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica. Per alcuni campi, ad esempio per i valori di valuta, è possibile che non si desideri modificare il simbolo di valuta nella casella di testo.

È possibile usare l'attributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) da solo, ma in genere è consigliabile usare anche l'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) . L'attributo `DataType` fornisce la *semantica* dei dati anziché come eseguirne il rendering in una schermata e offre i seguenti vantaggi che non si ottengono con `DisplayFormat`:

- Il browser può abilitare le funzionalità HTML5, ad esempio per visualizzare un controllo calendario, il simbolo di valuta appropriato per le impostazioni locali, i collegamenti alla posta elettronica e così via.
- Per impostazione predefinita, il browser eseguirà il rendering dei dati usando il formato corretto in base alle [impostazioni locali](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- L'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) può consentire a MVC di scegliere il modello di campo corretto per il rendering dei dati ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) se usato da solo USA il modello di stringa). Per altre informazioni, vedere i [modelli ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)di Brad Wilson. (Anche se scritto per MVC 2, questo articolo è ancora applicabile alla versione corrente di ASP.NET MVC).

Se si usa l'attributo `DataType` con un campo data, è necessario specificare anche l'attributo `DisplayFormat` per assicurarsi che il rendering del campo venga eseguito correttamente nei browser Chrome. Per altre informazioni, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Eseguire di nuovo la pagina di indice Student e notare che le ore non vengono più visualizzate per le date di registrazione. Lo stesso vale per qualsiasi visualizzazione che usa il modello di `Student`.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

È inoltre possibile specificare regole di convalida dei dati e messaggi utilizzando gli attributi. Ad esempio si supponga di voler limitare a 50 il numero massimo di caratteri che gli utenti possono immettere per un nome. Per aggiungere questa limitazione, aggiungere gli attributi [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) alle proprietà `LastName` e `FirstMidName`, come illustrato nell'esempio seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

L'attributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) non impedisce a un utente di immettere spazi vuoti per un nome. È possibile usare l'attributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) per applicare restrizioni all'input. Il codice seguente, ad esempio, richiede che il primo carattere sia maiuscolo e i caratteri rimanenti siano alfabetici:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

L'attributo [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) fornisce funzionalità simili all'attributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) , ma non fornisce la convalida lato client.

Eseguire l'applicazione e fare clic sulla scheda **students** . Viene ottenuto l'errore seguente:

*Il modello che supporta il contesto ' SchoolContext ' è stato modificato dopo la creazione del database. Si consiglia di utilizzare Migrazioni Code First per aggiornare il database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Il modello di database è stato modificato in modo da richiedere una modifica nello schema del database e Entity Framework rilevato. Si utilizzeranno le migrazioni per aggiornare lo schema senza perdere i dati aggiunti al database tramite l'interfaccia utente. Se sono stati modificati i dati creati dal metodo `Seed`, questo verrà modificato nuovamente allo stato originale a causa del metodo [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) in uso nel metodo `Seed`. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) è equivalente a un'operazione "upsert" della terminologia del database.)

Nella console di Gestione pacchetti immettere i comandi seguenti:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

Il `add-migration MaxLengthOnNames` comando crea un file denominato *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Questo file contiene il codice che aggiornerà il database in modo che corrisponda al modello di dati corrente. Il timestamp anteposto al nome del file di migrazione viene usato da Entity Framework per ordinare le migrazioni. Dopo aver creato più migrazioni, se si elimina il database o se si distribuisce il progetto utilizzando le migrazioni, tutte le migrazioni verranno applicate nell'ordine in cui sono state create.

Eseguire la pagina **Crea** e immettere il nome più lungo di 50 caratteri. Non appena si superano 50 caratteri, la convalida lato client visualizza immediatamente un messaggio di errore.

![errore Val lato client](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>Attributo Column

È possibile usare gli attributi anche per controllare il mapping delle classi e delle proprietà nel database. Si supponga di aver usato il nome `FirstMidName` per il campo first-name (Nome) perché il campo potrebbe contenere anche un secondo nome. Tuttavia si vuole che la colonna di database sia denominata `FirstName`, perché gli utenti che scrivono query ad hoc per il database sono abituati a tale nome. Per eseguire questo mapping è possibile usare l'attributo `Column`.

L'attributo `Column` specifica che quando viene creato il database, la colonna della tabella `Student` mappata sulla proprietà `FirstMidName` verrà denominata `FirstName`. In altri termini, quando il codice fa riferimento a `Student.FirstMidName` i dati provengono dalla colonna `FirstName` della tabella `Student` o vengono aggiornati in tale colonna. Se non si specificano nomi di colonna, viene assegnato lo stesso nome del nome della proprietà.

Aggiungere un'istruzione using per [System. ComponentModel. DataAnnotations. Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) e l'attributo Column Name alla proprietà `FirstMidName`, come illustrato nel codice evidenziato di seguito:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

L'aggiunta dell' [attributo Column](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) modifica il modello che supporta il schoolContext, in modo che non corrisponda al database. Immettere i comandi seguenti nella console di gestione pacchetti per creare un'altra migrazione:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

In **Esplora server** (**Esplora database** se si usa Express per il Web), fare doppio clic sulla tabella *Student* .

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Nell'immagine seguente viene illustrato il nome della colonna originale come prima dell'applicazione delle prime due migrazioni. Oltre al nome della colonna che passa da `FirstMidName` a `FirstName`, le due colonne nome sono state modificate da `MAX` lunghezza a 50 caratteri.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

È anche possibile apportare modifiche al mapping del database usando l' [API Fluent](https://msdn.microsoft.com/data/jj591617), come si vedrà più avanti in questa esercitazione.

> [!NOTE]
> Se si tenta di eseguire la compilazione prima di completare la creazione di tutte le classi di entità, è possibile che vengano generati errori del compilatore.

## <a name="create-the-instructor-entity"></a>Creare l'entità Instructor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Creare *Models\Instructor.cs*, sostituendo il codice del modello con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Si noti che molte proprietà sono uguali nelle entità `Student` e `Instructor`. Nell'esercitazione sull' [implementazione dell'ereditarietà](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) più avanti in questa serie si eseguirà il refactoring usando l'ereditarietà per eliminare questa ridondanza.

### <a name="the-required-and-display-attributes"></a>Attributi di visualizzazione e richiesti

Gli attributi della proprietà `LastName` specificano che si tratta di un campo obbligatorio, che la didascalia per la casella di testo deve essere "cognome" (anziché il nome della proprietà, ovvero "LastName" senza spazio) e che il valore non può essere più lungo di 50 caratteri.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

L' [attributo StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) imposta la lunghezza massima del database e fornisce la convalida lato client e lato server per ASP.NET MVC. È anche possibile specificare la lunghezza minima della stringa in questo attributo, ma il valore minimo non ha alcun effetto sullo schema del database. L' [attributo required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) non è necessario per i tipi di valore come DateTime, int, Double e float. Ai tipi di valore non è possibile assegnare un valore null, quindi sono intrinsecamente necessari. È possibile rimuovere l' [attributo obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) e sostituirlo con un parametro di lunghezza minima per l'attributo `StringLength`:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

È possibile inserire più attributi su una riga, quindi è anche possibile scrivere la classe Instructor come indicato di seguito:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>Proprietà calcolata FullName

`FullName` è una proprietà calcolata che restituisce un valore creato concatenando altre due proprietà. Pertanto dispone solo di una funzione di accesso `get` e non verrà generata alcuna colonna `FullName` nel database.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Proprietà di navigazione Courses e OfficeAssignment

Le proprietà `Courses` e `OfficeAssignment` sono proprietà di navigazione. Come spiegato in precedenza, vengono in genere definite come [virtuali](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) , in modo che possano sfruttare i vantaggi di una Entity Framework funzionalità denominata [caricamento lazy](https://msdn.microsoft.com/magazine/hh205756.aspx). Inoltre, se una proprietà di navigazione può ospitare più entità, il tipo deve implementare l'interfaccia [ICollection&lt;t&gt;](https://msdn.microsoft.com/library/92t2ye13.aspx) . Ad esempio, [IList&lt;t&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) qualifica ma non [IEnumerable&lt;t&gt;](https://msdn.microsoft.com/library/9eekhta0.aspx) perché `IEnumerable<T>` non implementa [Add](https://msdn.microsoft.com/library/63ywd54z.aspx).

Un insegnante può insegnare un numero qualsiasi di corsi, quindi `Courses` viene definito come una raccolta di entità `Course`. Lo stato delle regole aziendali un insegnante può avere al massimo un ufficio, pertanto `OfficeAssignment` viene definito come una singola entità `OfficeAssignment` (che può essere `null` se non è stato assegnato alcun ufficio).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Creare l'entità OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Creare *Models\OfficeAssignment.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Compilare il progetto, che consente di salvare le modifiche e verificare che non siano stati eseguiti errori di copia e incolla che il compilatore può rilevare.

### <a name="the-key-attribute"></a>Attributo chiave

Esiste una relazione uno-a-zero-o-uno tra le entità `Instructor` e `OfficeAssignment`. Un'assegnazione di ufficio esiste solo in relazione all'insegnante a cui è assegnata e pertanto la chiave primaria è anche la chiave esterna per l'entità `Instructor`. Tuttavia, il Entity Framework non è in grado di riconoscere automaticamente `InstructorID` come chiave primaria di questa entità perché il nome non segue la convenzione di denominazione `ID` o *nomeclasse* `ID`. Per identificare l'entità come chiave viene usato l'attributo `Key`:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

È anche possibile usare l'attributo `Key` se l'entità ha una propria chiave primaria, ma si vuole assegnare alla proprietà un nome diverso da `classnameID` o `ID`. Per impostazione predefinita, EF considera la chiave come non generata dal database, perché la colonna è destinata a una relazione di identificazione.

### <a name="the-foreignkey-attribute"></a>Attributo ForeignKey

Quando è presente una relazione uno-a-zero-o-uno o una relazione uno-a-uno tra due entità (ad esempio tra `OfficeAssignment` e `Instructor`), EF non riesce a risolvere quale estremità della relazione è il principale e quale entità finale è dipendente. Le relazioni uno-a-uno hanno una proprietà di navigazione di riferimento in ogni classe per l'altra classe. L' [attributo ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) può essere applicato alla classe dipendente per stabilire la relazione. Se si omette l' [attributo ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), viene ottenuto l'errore seguente quando si tenta di creare la migrazione:

Impossibile determinare l'entità finale principale di un'associazione tra i tipi "ContosoUniversity. Models. OfficeAssignment" e "ContosoUniversity. Models. Instructor". L'entità finale principale di questa associazione deve essere configurata in modo esplicito usando le annotazioni dei dati o dell'API Fluent della relazione.

Più avanti nell'esercitazione verrà illustrato come configurare questa relazione con l'API Fluent.

### <a name="the-instructor-navigation-property"></a>Proprietà di navigazione Instructor

L'entità `Instructor` dispone di una proprietà di navigazione `OfficeAssignment` Nullable (perché un insegnante potrebbe non avere un'assegnazione di ufficio) e l'entità `OfficeAssignment` ha una proprietà di navigazione `Instructor` che non ammette i valori null, perché un'assegnazione di ufficio non può esistere senza un insegnante, `InstructorID` non ammette i valori null. Quando un'entità `Instructor` dispone di un'entità `OfficeAssignment` correlata, ogni entità avrà un riferimento all'altra entità nella relativa proprietà di navigazione.

È possibile inserire un attributo `[Required]` nella proprietà di navigazione Instructor per specificare che deve essere presente un insegnante correlato, ma questa operazione non è necessaria perché la chiave esterna InstructorID, che è anche la chiave di questa tabella, non ammette i valori null.

## <a name="modify-the-course-entity"></a>Modificare l'entità Course

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

In *Models\Course.cs*sostituire il codice aggiunto in precedenza con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

L'entità Course include una proprietà di chiave esterna `DepartmentID` che fa riferimento all'entità `Department` correlata e dispone di una proprietà di navigazione `Department`. In Entity Framework non è necessario aggiungere una proprietà di chiave esterna al modello di dati se è disponibile una proprietà di navigazione per un'entità correlata. EF crea automaticamente chiavi esterne nel database ogni volta che sono necessarie. Tuttavia il fatto di avere la chiave esterna nel modello di dati può rendere più semplici ed efficienti gli aggiornamenti. Ad esempio, quando si recupera un'entità Course da modificare, l'entità `Department` è null se non viene caricata. Pertanto, quando si aggiorna l'entità Course, è necessario recuperare prima l'entità `Department`. Quando la proprietà di chiave esterna `DepartmentID` viene inclusa nel modello di dati, non è necessario recuperare l'entità `Department` prima dell'aggiornamento.

### <a name="the-databasegenerated-attribute"></a>Attributo DatabaseGenerated

L' [attributo DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) con il parametro [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) sulla proprietà `CourseID` specifica che i valori di chiave primaria vengono forniti dall'utente anziché generati dal database.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Per impostazione predefinita, il Entity Framework presuppone che i valori di chiave primaria vengano generati dal database. Questa è la condizione ottimale nella maggior parte degli scenari. Tuttavia, per `Course` entità, si userà un numero di corso specificato dall'utente, ad esempio una serie 1000 per un reparto, una serie 2000 per un altro reparto e così via.

### <a name="foreign-key-and-navigation-properties"></a>Proprietà di chiave esterna e di navigazione

Le proprietà di chiave esterna e le proprietà di navigazione nell'entità `Course` riflettono le relazioni seguenti:

- Un corso viene assegnato a un solo reparto, pertanto sono presenti una chiave esterna `DepartmentID` e una proprietà di navigazione `Department` per i motivi indicati in precedenza. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Un corso può avere un numero qualsiasi di studenti iscritti, pertanto la proprietà di navigazione `Enrollments` è una raccolta: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Un corso può essere impartito da più insegnanti, pertanto la proprietà di navigazione `Instructors` è una raccolta: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Creazione dell'entità Department

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Creare *Models\Department.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Attributo Column

In precedenza è stato utilizzato l' [attributo Column](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) per modificare il mapping del nome di colonna. Nel codice per l'entità `Department`, l'attributo `Column` viene utilizzato per modificare il mapping dei tipi di dati SQL in modo che la colonna venga definita utilizzando il tipo di SQL Server [Money](https://msdn.microsoft.com/library/ms179882.aspx) nel database:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Il mapping delle colonne non è in genere necessario, perché il Entity Framework di solito sceglie il tipo di dati SQL Server appropriato in base al tipo CLR definito per la proprietà. Il tipo CLR `decimal` esegue il mapping a un tipo SQL Server `decimal`. Tuttavia, in questo caso, si sa che la colonna conterrà importi di valuta e il tipo di dati [Money](https://msdn.microsoft.com/library/ms179882.aspx) è più appropriato.

### <a name="foreign-key-and-navigation-properties"></a>Proprietà di chiave esterna e di navigazione

Le proprietà di chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:

- Un reparto può avere o meno un amministratore e un amministratore è sempre un insegnante. Pertanto la proprietà `InstructorID` viene inclusa come chiave esterna per l'entità `Instructor` e viene aggiunto un punto interrogativo dopo la designazione del tipo di `int` per contrassegnare la proprietà come Nullable. La proprietà di navigazione è denominata `Administrator` ma include un'entità `Instructor`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Un reparto può avere molti corsi, quindi è presente una proprietà di navigazione `Courses`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Per convenzione, Entity Framework consente l'eliminazione a catena per le chiavi esterne non nullable e per le relazioni molti-a-molti. Ciò può comportare regole di eliminazione a catena circolari, che generano un'eccezione quando viene eseguito il codice dell'inizializzatore. Se, ad esempio, la proprietà `Department.InstructorID` non è stata definita come Nullable, durante l'esecuzione dell'inizializzatore verrà visualizzato il messaggio di eccezione seguente: "la relazione referenziale genera un riferimento ciclico non consentito". Se le regole business richiedono `InstructorID` proprietà come non nullable, è necessario usare l'API Fluent seguente per disabilitare l'eliminazione a catena sulla relazione: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modifying-the-student-entity"></a>Modifica dell'entità Student

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

In *Models\Student.cs*sostituire il codice aggiunto in precedenza con il codice seguente. Le modifiche vengono evidenziate.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>Entità di registrazione

 In *Models\Enrollment.cs*sostituire il codice aggiunto in precedenza con il codice seguente

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Proprietà di chiave esterna e di navigazione

Le proprietà di chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:

- Un record di iscrizione è relativo a un singolo corso, pertanto sono presenti una proprietà di chiave esterna `CourseID` e una proprietà di navigazione `Course`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Un record di iscrizione è relativo a un singolo studente, pertanto sono presenti una proprietà di chiave esterna `StudentID` e una proprietà di navigazione `Student`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Relazioni molti-a-molti

Esiste una relazione molti-a-molti tra le entità `Student` e `Course` e l'entità `Enrollment` funziona come una tabella di join molti-a-molti *con payload* nel database. Ciò significa che la tabella `Enrollment` contiene dati aggiuntivi oltre alle chiavi esterne per le tabelle unite in join, in questo caso una chiave primaria e una proprietà di `Grade`.

La figura seguente illustra l'aspetto di queste relazioni in un diagramma di entità. (Questo diagramma è stato generato usando il [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d). la creazione del diagramma non fa parte dell'esercitazione, ma viene usata qui come illustrazione).

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Ogni linea di relazione ha un valore 1 a un'estremità e un asterisco (\*) nell'altra, che indica una relazione uno-a-molti.

Se nella tabella `Enrollment` non sono incluse informazioni sul livello, è necessario che contengano solo le due chiavi esterne `CourseID` e `StudentID`. In tal caso, corrisponderebbe a una tabella di join molti-a-molti *senza payload* (o una *tabella di join pura*) nel database e non sarebbe necessario creare una classe di modello. Le entità `Instructor` e `Course` hanno questo tipo di relazione molti-a-molti e come si può notare, non esiste alcuna classe di entità tra di esse:

![Insegnante-Course_many-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Tuttavia, nel database è necessaria una tabella di join, come illustrato nel diagramma di database seguente:

![Insegnante-Course_many-many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Il Entity Framework crea automaticamente la tabella `CourseInstructor`, che viene letta e aggiornata indirettamente leggendo e aggiornando le proprietà di navigazione `Instructor.Courses` e `Course.Instructors`.

## <a name="entity-diagram-showing-relationships"></a>Diagramma dell'entità che visualizza le relazioni

La figura seguente visualizza il diagramma creato da Entity Framework Power Tools per il modello School completato.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Oltre alle linee delle relazioni molti-a-molti (\* \*) e alle linee delle relazioni uno-a-molti (da 1 a \*), qui è possibile vedere la linea di relazione uno-a-zero-o-uno (da 1 a 0.. 1) tra le entità `Instructor` e `OfficeAssignment` e la riga di relazione zero-o-uno-a-molti (da 0.. 1 a \*) tra le entità Instructor e Department.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Personalizzare il modello di dati aggiungendo codice al contesto del database

Aggiungere quindi le nuove entità alla classe `SchoolContext` e personalizzare parte del mapping usando le chiamate [API Fluent](https://msdn.microsoft.com/data/jj591617) . L'API è "Fluent" perché viene spesso usata mediante la stringa di una serie di chiamate al metodo in un'unica istruzione.

In questa esercitazione si userà l'API Fluent solo per il mapping del database che non è possibile eseguire con gli attributi. È tuttavia possibile usare l'API Fluent anche per specificare la maggior parte delle regole di formattazione, convalida e mapping specificabili tramite gli attributi. Alcuni attributi quali `MinimumLength` non possono essere applicati con l'API Fluent. Come indicato in precedenza, `MinimumLength` non modifica lo schema, applica solo una regola di convalida lato client e server

Alcuni sviluppatori preferiscono usare solo l'API Fluent, per dare un aspetto "ordinato" alle classi di entità. È possibile usare un misto di attributi e API Fluent e alcune personalizzazioni sono eseguibili solo mediante l'API Fluent, ma in generale è consigliabile scegliere uno dei due approcci e usarlo in modo il più possibile coerente.

Per aggiungere le nuove entità al modello di dati ed eseguire il mapping del database che non è stato fatto usando gli attributi, sostituire il codice in *DAL\SchoolContext.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

La nuova istruzione nel metodo [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) configura la tabella di join molti-a-molti:

- Per la relazione molti-a-molti tra le entità `Instructor` e `Course`, il codice specifica i nomi di tabella e colonna per la tabella di join. Code First possibile configurare la relazione molti-a-molti senza questo codice, ma se non viene chiamata, si otterranno nomi predefiniti, ad esempio `InstructorInstructorID` per la colonna `InstructorID`.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Il codice seguente fornisce un esempio di come è possibile usare l'API Fluent anziché gli attributi per specificare la relazione tra le entità `Instructor` e `OfficeAssignment`:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Per informazioni sulle istruzioni "API Fluent" eseguite dietro le quinte, vedere il post di Blog sull' [API Fluent](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) .

## <a name="seed-the-database-with-test-data"></a>Assegnare valori di inizializzazione al database con dati di test

Sostituire il codice nel file *Migrations\Configuration.cs* con il codice seguente per fornire i dati di inizializzazione per le nuove entità create.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Come illustrato nella prima esercitazione, la maggior parte di questo codice aggiorna o crea nuovi oggetti entità e carica i dati di esempio in proprietà come richiesto per i test. Si noti tuttavia il modo in cui viene gestita l'entità `Course`, che ha una relazione molti-a-molti con l'entità `Instructor`:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Quando si crea un oggetto `Course`, si inizializza la proprietà di navigazione `Instructors` come raccolta vuota utilizzando il `Instructors = new List<Instructor>()`di codice. In questo modo è possibile aggiungere `Instructor` entità correlate a questo `Course` utilizzando il metodo `Instructors.Add`. Se non è stato creato un elenco vuoto, non sarà possibile aggiungere queste relazioni, perché la proprietà `Instructors` sarà null e non avrà un metodo di `Add`. È anche possibile aggiungere l'inizializzazione dell'elenco al costruttore.

## <a name="add-a-migration-and-update-the-database"></a>Aggiungere una migrazione e aggiornare il database

Dalla console di gestione pacchetti immettere il comando `add-migration`:

`PM> add-Migration Chap4`

Se si tenta di aggiornare il database a questo punto, si riceverà l'errore seguente:

*L'istruzione ALTER TABLE è in conflitto con il vincolo FOREIGN KEY "FK\_dbo. Corso\_dbo. Reparto\_DepartmentID ". Il conflitto si è verificato nel database "ContosoUniversity", tabella "dbo". Department ", colonna ' DepartmentID '.*

Modificare il &lt;*timestamp&gt;\_file Chap4.cs* e apportare le modifiche al codice seguenti (verrà aggiunta un'istruzione SQL e verrà modificata un'istruzione `AddColumn`):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Assicurarsi di impostare come commento o eliminare la riga di `AddColumn` esistente quando si aggiunge la nuova riga o si riceve un errore quando si immette il comando `update-database`.

In alcuni casi, quando si eseguono migrazioni con dati esistenti, è necessario inserire i dati dello stub nel database per soddisfare i vincoli di chiave esterna e questo è quello che si sta facendo. Il codice generato aggiunge una chiave esterna `DepartmentID` non nullable alla tabella `Course`. Se sono già presenti righe nella tabella `Course` durante l'esecuzione del codice, l'operazione di `AddColumn` non riuscirà perché SQL Server non conosce il valore da inserire nella colonna che non può essere null. Pertanto è stato modificato il codice in modo da assegnare alla nuova colonna un valore predefinito ed è stato creato un reparto Stub denominato "Temp" che funge da reparto predefinito. Di conseguenza, se sono presenti righe `Course` durante l'esecuzione del codice, saranno tutte correlate al reparto "Temp".

Quando viene eseguito il `Seed` metodo, verranno inserite righe nella tabella `Department` e verranno correlate le righe `Course` esistenti alle nuove righe `Department`. Se non sono stati aggiunti corsi nell'interfaccia utente, non sarà più necessario il reparto "Temp" o il valore predefinito nella colonna `Course.DepartmentID`. Per consentire a un utente di aggiungere corsi utilizzando l'applicazione, è necessario aggiornare anche il codice del metodo `Seed` per assicurarsi che tutte le `Course` righe (non solo quelle inserite dalle esecuzioni precedenti del metodo di `Seed`) abbiano valori `DepartmentID` validi prima di rimuovere il valore predefinito dalla colonna ed eliminare il reparto "Temp".

Al termine della modifica del &lt;*timestamp&gt;\_file Chap4.cs* , immettere il comando `update-database` nella console di gestione pacchetti per eseguire la migrazione.

> [!NOTE]
> È possibile che si verifichino altri errori durante la migrazione dei dati e l'esecuzione di modifiche dello schema. Se si verificano errori di migrazione che non possono essere risolti, è possibile modificare la stringa di connessione nel file *Web. config* o eliminare il database. L'approccio più semplice consiste nel rinominare il database nel file *Web. config* . Ad esempio, modificare il nome del database in CU\_test come illustrato di seguito:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
> Con un nuovo database, non sono presenti dati da migrare e il `update-database` comando è molto più probabile che venga completato senza errori. Per istruzioni su come eliminare il database, vedere [come rimuovere un database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).

Aprire il database in **Esplora server** come è stato fatto in precedenza ed espandere il nodo **tabelle** per verificare che tutte le tabelle siano state create. Se ancora **Esplora server** aperto dall'ora precedente, fare clic sul pulsante **Aggiorna** .

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Non è stata creata una classe modello per la tabella `CourseInstructor`. Come spiegato in precedenza, si tratta di una tabella di join per la relazione molti-a-molti tra le entità `Instructor` e `Course`.

Fare clic con il pulsante destro del mouse sulla tabella `CourseInstructor` e scegliere **Mostra dati tabella** per verificare che siano presenti dati come risultato delle entità `Instructor` aggiunte alla proprietà di navigazione `Course.Instructors`.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Riepilogo

Ora sono presenti un modello di dati più complesso e il database corrispondente. Nell'esercitazione seguente verranno fornite ulteriori informazioni sui diversi modi per accedere ai dati correlati.

I collegamenti ad altre risorse Entity Framework sono disponibili nella [mappa del contenuto di ASP.NET Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
