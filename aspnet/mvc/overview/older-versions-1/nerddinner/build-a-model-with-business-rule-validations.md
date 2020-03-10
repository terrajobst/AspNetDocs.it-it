---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Compilare un modello con le convalide delle regole business | Microsoft Docs
author: microsoft
description: Il passaggio 3 Mostra come creare un modello che è possibile usare per eseguire query e aggiornare il database per l'applicazione NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 6ebf1b71c089229ba9139ff7dc788b8978724046
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541664"
---
# <a name="build-a-model-with-business-rule-validations"></a>Creare un modello con convalide delle regole business

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 3 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni, ma completa, usando ASP.NET MVC 1.
> 
> Il passaggio 3 Mostra come creare un modello che è possibile usare per eseguire query e aggiornare il database per l'applicazione NerdDinner.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner passaggio 3: compilazione del modello

In un framework MVC (Model-View-Controller) il termine "modello" si riferisce agli oggetti che rappresentano i dati dell'applicazione, nonché alla logica di dominio corrispondente che integra la convalida e le regole business. Il modello è in molti modi il "cuore" di un'applicazione basata su MVC e, come si vedrà in seguito, ne determina il comportamento.

Il framework ASP.NET MVC supporta l'utilizzo di qualsiasi tecnologia di accesso ai dati e gli sviluppatori possono scegliere tra una vasta gamma di opzioni di dati .NET avanzate per implementare i propri modelli, tra cui: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, Subsonic, WilsonORM o solo ADO non elaborato. NET DataReaders o set di impostazioni.

Per l'applicazione NerdDinner verrà usato LINQ to SQL per creare un modello semplice che corrisponda in modo abbastanza accurato alla progettazione del database e aggiunge alcune regole di business e logica di convalida personalizzata. Verrà quindi implementata una classe di repository che consente di astrarre l'implementazione di persistenza dei dati dal resto dell'applicazione e di unit testrla facilmente.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL è un ORM (Object Relational Mapper) che viene fornito come parte di .NET 3,5.

LINQ to SQL fornisce un modo semplice per eseguire il mapping delle tabelle di database alle classi .NET che è possibile codificare. Per l'applicazione NerdDinner verrà usato per eseguire il mapping delle cene e delle tabelle RSVP nel database alle classi Dinner e RSVP. Le colonne delle cene e delle tabelle RSVP corrispondono alle proprietà delle classi Dinner e RSVP. Ogni oggetto Dinner e RSVP rappresenterà una riga separata all'interno delle cene o delle tabelle RSVP nel database.

LINQ to SQL consente di evitare di dover creare manualmente istruzioni SQL per recuperare e aggiornare gli oggetti Dinner e RSVP con i dati del database. Verranno invece definite le classi Dinner e RSVP, il modo in cui eseguono il mapping al/dal database e le relazioni tra di essi. LINQ to SQL si occuperà quindi di generare la logica di esecuzione di SQL appropriata da usare in fase di esecuzione quando vengono interagite e utilizzate.

È possibile usare il supporto del linguaggio LINQ in VB C# e scrivere query espressive che recuperano oggetti Dinner e RSVP dal database. Questa operazione consente di ridurre al minimo la quantità di codice dati che è necessario scrivere e di creare applicazioni effettivamente pulite.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Aggiunta di LINQ to SQL classi al progetto

Per iniziare, fare clic con il pulsante destro del mouse sulla cartella "modelli" all'interno del progetto e selezionare il comando di menu **aggiungi&gt;nuovo elemento** :

![](build-a-model-with-business-rule-validations/_static/image1.png)

Verrà visualizzata la finestra di dialogo "Aggiungi nuovo elemento". Si filtra in base alla categoria "data" e si seleziona il modello "LINQ to SQL Classes" al suo interno:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Assegnare un nome all'elemento "NerdDinner" e fare clic sul pulsante "Aggiungi". In Visual Studio verrà aggiunto un file NerdDinner. dbml nella directory \Models e verrà aperto il LINQ to SQL progettazione relazionale oggetti:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Creazione di classi di modelli di dati con LINQ to SQL

LINQ to SQL consente di creare rapidamente classi del modello di dati dallo schema di database esistente. A tale scopo, si aprirà il database NerdDinner nel Esplora server e si selezioneranno le tabelle che si desidera modellare:

![](build-a-model-with-business-rule-validations/_static/image4.png)

È quindi possibile trascinare le tabelle nell'area di progettazione LINQ to SQL. Quando si esegue questa operazione LINQ to SQL creerà automaticamente le classi Dinner e RSVP usando lo schema delle tabelle (con le proprietà della classe che eseguono il mapping alle colonne della tabella di database):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Per impostazione predefinita, la finestra di progettazione LINQ to SQL "plurali" automaticamente i nomi di tabella e colonna quando crea classi basate su uno schema di database. Ad esempio, la tabella "dinners" nell'esempio precedente ha prodotto una classe "Dinner". Questa denominazione della classe consente di rendere i modelli coerenti con le convenzioni di denominazione .NET e di solito si ritiene che la finestra di progettazione possa risolvere questo problema, soprattutto quando si aggiungono molte tabelle. Se non si preferisce il nome di una classe o di una proprietà generata dalla finestra di progettazione, è comunque possibile eseguirne l'override e sostituirla con il nome desiderato. Per eseguire questa operazione, è possibile modificare il nome dell'entità o della proprietà all'interno della finestra di progettazione o modificarlo tramite la griglia delle proprietà.

Per impostazione predefinita, la finestra di progettazione di LINQ to SQL controlla anche le relazioni di chiave primaria/chiave esterna delle tabelle e in base a esse crea automaticamente le "associazioni di relazioni" predefinite tra le diverse classi di modello create. Ad esempio, quando le cene e le tabelle RSVP sono state trascinate nella finestra di progettazione di LINQ to SQL, un'associazione di relazioni uno-a-molti tra i due è stata dedotta in base al fatto che la tabella RSVP aveva una chiave esterna per la tabella Dinners (indicata dalla freccia nel finestra di progettazione:

![](build-a-model-with-business-rule-validations/_static/image6.png)

L'associazione precedente comporterà LINQ to SQL aggiungere una proprietà "Dinner" fortemente tipizzata alla classe RSVP che gli sviluppatori possono usare per accedere alla cena associata a un dato RSVP. Inoltre, la classe Dinner avrà una proprietà della raccolta "RSVPs" che consente agli sviluppatori di recuperare e aggiornare gli oggetti RSVP associati a una particolare cena.

Di seguito è possibile vedere un esempio di IntelliSense in Visual Studio quando si crea un nuovo oggetto RSVP e lo si aggiunge alla raccolta RSVPs di Dinner. Si noti come LINQ to SQL aggiunto automaticamente una raccolta "RSVPs" nell'oggetto Dinner:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Aggiungendo l'oggetto RSVP alla raccolta RSVPs di Dinner, viene indicato LINQ to SQL di associare una relazione di chiave esterna tra il dinner e la riga RSVP nel database:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Se non si desidera che la finestra di progettazione abbia modellato o denominato un'associazione di tabella, è possibile eseguirne l'override. È sufficiente fare clic sulla freccia di associazione all'interno della finestra di progettazione e accedere alle relative proprietà tramite la griglia delle proprietà per rinominarla, eliminarla o modificarla. Per l'applicazione NerdDinner, tuttavia, le regole di associazione predefinite funzionano correttamente per le classi del modello di dati che si stanno compilando ed è possibile usare solo il comportamento predefinito.

### <a name="nerddinnerdatacontext-class"></a>Classe NerdDinnerDataContext

Visual Studio creerà automaticamente le classi .NET che rappresentano i modelli e le relazioni di database definiti utilizzando la finestra di progettazione LINQ to SQL. Viene inoltre generata una classe LINQ to SQL DataContext per ogni file di LINQ to SQL Designer aggiunto alla soluzione. Poiché è stato denominato l'elemento della classe LINQ to SQL "NerdDinner", la classe DataContext creata verrà chiamata "NerdDinnerDataContext". Questa classe NerdDinnerDataContext è la modalità principale per interagire con il database.

La classe NerdDinnerDataContext espone due proprietà, ovvero "dinners" e "RSVPs", che rappresentano le due tabelle modellate all'interno del database. È possibile utilizzare C# per scrivere query LINQ su tali proprietà per eseguire query e recuperare oggetti Dinner e RSVP dal database.

Nel codice seguente viene illustrato come creare un'istanza di un oggetto NerdDinnerDataContext ed eseguire una query LINQ su di esso per ottenere una sequenza di cene che si verificano in futuro. Visual Studio fornisce IntelliSense completo durante la scrittura della query LINQ e gli oggetti restituiti sono fortemente tipizzati e supportano anche IntelliSense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Oltre a consentire l'esecuzione di query per gli oggetti Dinner e RSVP, un NerdDinnerDataContext tiene traccia automaticamente delle modifiche apportate successivamente agli oggetti Dinner e RSVP recuperati. È possibile usare questa funzionalità per salvare facilmente le modifiche nel database, senza dover scrivere codice di aggiornamento SQL esplicito.

Ad esempio, il codice seguente illustra come usare una query LINQ per recuperare un singolo oggetto dinner dal database, aggiornare due delle proprietà Dinner e quindi salvare le modifiche nel database:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

L'oggetto NerdDinnerDataContext nel codice precedente rileva automaticamente le modifiche alle proprietà apportate all'oggetto Dinner recuperato. Quando è stato chiamato il metodo "SubmitChanges ()", verrà eseguita un'istruzione SQL "UPDATE" appropriata nel database per salvare di nuovo i valori aggiornati.

### <a name="creating-a-dinnerrepository-class"></a>Creazione di una classe DinnerRepository

Per le piccole applicazioni è talvolta possibile che i controller funzionino direttamente con una classe LINQ to SQL DataContext e incorporano query LINQ all'interno dei controller. Poiché le applicazioni diventano più grandi, tuttavia, questo approccio diventa complesso da gestire e testare. Può anche comportare la duplicazione delle stesse query LINQ in più posizioni.

Uno degli approcci che semplificano la gestione e il test delle applicazioni consiste nell'usare un modello di "repository". Una classe di repository consente di incapsulare le query e la logica di persistenza dei dati ed estrae i dettagli di implementazione della persistenza dei dati dall'applicazione. Oltre a rendere più pulito il codice dell'applicazione, l'uso di un modello di repository può semplificare la modifica delle implementazioni di archiviazione dei dati in futuro e consente di facilitare il testing unità di un'applicazione senza richiedere un database reale.

Per l'applicazione NerdDinner verrà definita una classe DinnerRepository con la firma seguente:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Nota: più avanti in questo capitolo si estrae un'interfaccia IDinnerRepository da questa classe e si Abilita l'inserimento delle dipendenze nei controller. Per iniziare, tuttavia, l'avvio sarà semplice e funzionerà direttamente con la classe DinnerRepository.*

Per implementare questa classe, fare clic con il pulsante destro del mouse sulla cartella "Models" e scegliere il comando di menu **aggiungi&gt;nuovo elemento** . Nella finestra di dialogo "Aggiungi nuovo elemento" selezionare il modello "classe" e denominare il file "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

È quindi possibile implementare la classe DinnerRepository usando il codice seguente:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Recupero, aggiornamento, inserimento ed eliminazione mediante la classe DinnerRepository

Ora che è stata creata la classe DinnerRepository, verranno esaminati alcuni esempi di codice che illustrano le attività comuni che è possibile eseguire:

#### <a name="querying-examples"></a>Esempi di query

Il codice seguente recupera una singola cena usando il valore DinnerID:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Il codice seguente recupera tutte le cene e i cicli successivi:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Esempi di inserimento e aggiornamento

Il codice seguente illustra l'aggiunta di due nuove cene. Le aggiunte/modifiche al repository non vengono sottoposte a commit nel database fino a quando non viene chiamato il metodo "Save ()". LINQ to SQL esegue automaticamente il wrapping di tutte le modifiche in una transazione di database, in modo che tutte le modifiche siano eseguite o nessuna di esse quando il repository Salva:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Il codice riportato di seguito consente di recuperare un oggetto Dinner esistente e di modificarvi due proprietà. Viene eseguito il commit delle modifiche nel database quando viene chiamato il metodo "Save ()" nel repository:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Il codice riportato di seguito consente di recuperare una cena e quindi di aggiungervi un RSVP. Questa operazione viene eseguita usando la raccolta RSVPs nell'oggetto Dinner che LINQ to SQL creato per noi (perché esiste una relazione chiave primaria/chiave esterna tra i due nel database). Questa modifica viene salvata in modo permanente nel database come nuova riga della tabella RSVP quando il metodo "Save ()" viene chiamato nel repository:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Elimina esempio

Il codice seguente recupera un oggetto Dinner esistente, quindi lo contrassegna per l'eliminazione. Quando il metodo "Save ()" viene chiamato sul repository, eseguirà il commit dell'operazione di eliminazione nel database:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integrazione della logica della regola business e della convalida con le classi del modello

L'integrazione della logica della regola business e della convalida è una parte essenziale di qualsiasi applicazione che funzioni con i dati.

#### <a name="schema-validation"></a>Convalida dello schema

Quando le classi del modello vengono definite utilizzando la finestra di progettazione LINQ to SQL, i tipi di dati delle proprietà nelle classi del modello di dati corrispondono ai tipi di dati della tabella di database. Ad esempio, se la colonna "elemento eventdate" nella tabella dinners è "DateTime", la classe del modello di dati creata da LINQ to SQL sarà di tipo "DateTime" (che è un tipo di dati .NET incorporato). Ciò significa che si otterranno errori di compilazione se si tenta di assegnare un numero intero o un valore booleano al codice e viene generato automaticamente un errore se si tenta di convertire in modo implicito un tipo di stringa non valido in fase di esecuzione.

LINQ to SQL gestisce automaticamente anche l'escape dei valori SQL quando si usano le stringhe, che consente di proteggersi dagli attacchi intrusivi nel codice SQL quando viene usato.

#### <a name="validation-and-business-rule-logic"></a>Convalida e logica delle regole business

La convalida dello schema è utile come primo passaggio, ma è raramente sufficiente. La maggior parte degli scenari reali richiede la possibilità di specificare una logica di convalida più completa che può estendersi su più proprietà, eseguire codice e spesso conoscere lo stato di un modello (ad esempio, viene creato/updated/Deleted o all'interno di uno stato specifico di dominio come "archiviato"). Sono disponibili diversi modelli e Framework che possono essere usati per definire e applicare regole di convalida alle classi di modelli. Esistono diversi framework basati su .NET che possono essere usati per semplificare questa operazione. È possibile usare quasi tutti gli elementi all'interno delle applicazioni MVC ASP.NET.

Ai fini dell'applicazione NerdDinner, si userà un modello relativamente semplice e diretto in cui vengono esposte una proprietà IsValid e un metodo GetRuleViolations () nell'oggetto modello dinner. La proprietà IsValid restituirà true o false a seconda che la convalida e le regole business siano valide. Il metodo GetRuleViolations () restituirà un elenco di eventuali errori di regola.

Verranno implementati IsValid e GetRuleViolations () per il modello Dinner aggiungendo una "classe parziale" al progetto. Le classi parziali possono essere utilizzate per aggiungere metodi/proprietà/eventi alle classi gestite da una finestra di progettazione di Visual Studio (ad esempio la classe Dinner generata dalla finestra di progettazione LINQ to SQL) e contribuire a evitare che lo strumento venga confuso con il codice. È possibile aggiungere una nuova classe parziale al progetto facendo clic con il pulsante destro del mouse sulla cartella \Models e quindi scegliendo il comando di menu "Aggiungi nuovo elemento". È quindi possibile scegliere il modello "class" nella finestra di dialogo "Aggiungi nuovo elemento" e denominarlo Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Quando si fa clic sul pulsante "Aggiungi", si aggiunge un file Dinner.cs al progetto e lo si apre nell'IDE. È quindi possibile implementare un Framework di applicazione di regole/convalida di base usando il codice seguente:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Di seguito sono riportate alcune note sul codice riportato sopra:

- La classe Dinner è preceduta da una parola chiave "parziale", il che significa che il codice contenuto al suo interno verrà combinato con la classe generata/gestita dalla finestra di progettazione LINQ to SQL e compilata in un'unica classe.
- La classe RuleViolation è una classe helper che verrà aggiunta al progetto che consente di fornire ulteriori dettagli sulla violazione di una regola.
- Il metodo dinner. GetRuleViolations () determina la valutazione delle regole di business e di convalida (verranno implementate a breve). Restituisce quindi una sequenza di oggetti RuleViolation che forniscono ulteriori dettagli sugli errori delle regole.
- La proprietà dinner. IsValid fornisce una comoda proprietà helper che indica se l'oggetto Dinner dispone di RuleViolations attivi. Può essere controllato in modo proattivo da uno sviluppatore che usa l'oggetto Dinner in qualsiasi momento (e non genera un'eccezione).
- Il metodo parziale dinner. OnValidate () è un hook che LINQ to SQL fornisce che consente di ricevere una notifica ogni volta che l'oggetto Dinner sta per essere reso permanente all'interno del database. L'implementazione OnValidate () precedente garantisce che la cena non disponga di RuleViolations prima di essere salvata. Se è in uno stato non valido, genera un'eccezione, che determina l'interruzione della transazione da parte di LINQ to SQL.

Questo approccio fornisce un framework semplice in cui è possibile integrare la convalida e le regole business in. Per il momento, aggiungere le regole seguenti al metodo GetRuleViolations ():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Si sta usando la funzionalità "yield return" di C# per restituire una sequenza di qualsiasi RuleViolations. I primi sei controlli di regola sopra impongono semplicemente che le proprietà della stringa nella cena non possono essere null o vuote. L'ultima regola è un po' più interessante e chiama un metodo helper PhoneValidator. IsValidNumber () che è possibile aggiungere al progetto per verificare che il formato del numero ContactPhone corrisponda al paese della cena.

È possibile usare. Supporto delle espressioni regolari di NET per implementare questo supporto per la convalida del telefono. Di seguito è riportata una semplice implementazione di PhoneValidator che è possibile aggiungere al progetto che consente di aggiungere controlli del modello Regex specifici del paese:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Gestione delle violazioni della convalida e della logica di business

Ora che è stato aggiunto il codice di convalida e di regola business precedente, ogni volta che si tenta di creare o aggiornare una cena, le regole della logica di convalida verranno valutate e applicate.

Gli sviluppatori possono scrivere codice come riportato di seguito per determinare in modo proattivo se un oggetto Dinner è valido e recuperare un elenco di tutte le violazioni senza generare eccezioni:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Se si tenta di salvare una cena in uno stato non valido, viene generata un'eccezione quando si chiama il metodo Save () su DinnerRepository. Questo problema si verifica perché LINQ to SQL chiama automaticamente il metodo parziale dinner. OnValidate () prima di salvare le modifiche della cena e abbiamo aggiunto il codice a dinner. OnValidate () per generare un'eccezione se nella cena sono presenti violazioni delle regole. Possiamo intercettare questa eccezione e recuperare in modo riattivo un elenco delle violazioni da correggere:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Poiché le regole business e di convalida sono implementate all'interno del livello del modello e non all'interno del livello dell'interfaccia utente, verranno applicate e utilizzate in tutti gli scenari all'interno dell'applicazione. In un secondo momento è possibile modificare o aggiungere regole business e fare in modo che tutto il codice che funziona con gli oggetti Dinner venga rispettato.

La flessibilità di modificare le regole di business in un'unica posizione, senza che tali modifiche rippleno in tutta l'applicazione e la logica dell'interfaccia utente, è un segno di un'applicazione ben scritta e un vantaggio che un framework MVC contribuisce a incoraggiare.

### <a name="next-step"></a>Passo successivo

È ora disponibile un modello che è possibile usare per eseguire query e aggiornare il database.

A questo punto, aggiungere alcuni controller e visualizzazioni al progetto che è possibile usare per creare un'esperienza dell'interfaccia utente HTML.

> [!div class="step-by-step"]
> [Precedente](create-a-database.md)
> [Successivo](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
