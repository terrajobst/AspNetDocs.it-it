---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Abilitare il testing unità automatizzato | Microsoft Docs
author: microsoft
description: Il passaggio 12 Mostra come sviluppare una suite di unit test automatizzati che verificano la funzionalità NerdDinner e che ci consentirà di apportare modifiche...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 09a7aa186605a6cce48ee94028425ded957c00d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541678"
---
# <a name="enable-automated-unit-testing"></a>Abilitare unit test automatici

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 12 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.
> 
> Il passaggio 12 Mostra come sviluppare una suite di unit test automatizzati che verificano la funzionalità NerdDinner e che ci consentirà di apportare modifiche e miglioramenti all'applicazione in futuro.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner passaggio 12: unit test

Viene ora sviluppato un gruppo di unit test automatizzati che verificano la funzionalità NerdDinner e che ci consentirà di apportare modifiche e miglioramenti all'applicazione in futuro.

### <a name="why-unit-test"></a>Perché unit test?

Nell'unità di lavoro una mattina si ha un improvviso lampo di ispirazione su un'applicazione su cui si sta lavorando. Ci si rende conto che è possibile implementare una modifica che renderà l'applicazione decisamente migliore. Potrebbe trattarsi di un refactoring che pulisce il codice, aggiunge una nuova funzionalità o corregge un bug.

La domanda che si verifica quando si arriva al computer è la seguente: "Qual è la sicurezza di questo miglioramento?" Cosa accade se la modifica ha effetti collaterali o si interrompe qualcosa? La modifica potrebbe essere semplice e richiedere pochi minuti per l'implementazione, ma cosa accade se sono necessarie ore per testare manualmente tutti gli scenari di applicazione? Cosa accade se si dimentica di coprire uno scenario e un'applicazione interruppe entra in produzione? Questo miglioramento è effettivamente degno di tutto il lavoro richiesto?

Gli unit test automatici possono fornire una rete di sicurezza che consente di migliorare continuamente le applicazioni ed evitare di temere il codice su cui si sta lavorando. La presenza di test automatizzati che verificano rapidamente la funzionalità consente di eseguire il codice in tutta sicurezza e di apportare miglioramenti che altrimenti potrebbero non essere più confortevoli. Consentono inoltre di creare soluzioni più gestibili e di una durata più lunga, il che comporta un ritorno molto più elevato sugli investimenti.

Il framework MVC ASP.NET rende semplice e naturale unit test la funzionalità dell'applicazione. Consente inoltre un flusso di lavoro di sviluppo basato su test (TDD) che consente lo sviluppo basato su test.

### <a name="nerddinnertests-project"></a>Progetto NerdDinner. tests

Quando è stata creata l'applicazione NerdDinner all'inizio di questa esercitazione, viene visualizzata una finestra di dialogo in cui viene chiesto se si desidera creare un progetto unit test da usare con il progetto di applicazione:

![](enable-automated-unit-testing/_static/image1.png)

È stato selezionato il pulsante di opzione "Sì, crea un progetto unit test", che ha determinato l'aggiunta di un progetto "NerdDinner. tests" alla soluzione:

![](enable-automated-unit-testing/_static/image2.png)

Il progetto NerdDinner. tests fa riferimento all'assembly del progetto di applicazione NerdDinner e consente di aggiungere facilmente test automatizzati per verificare la funzionalità dell'applicazione.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Creazione di unit test per la classe del modello Dinner

È ora possibile aggiungere alcuni test al progetto NerdDinner. tests che verificano la classe Dinner creata durante la compilazione del livello del modello.

Si inizierà creando una nuova cartella all'interno del progetto di test denominato "Models", in cui verranno posizionati i test correlati al modello. Fare clic con il pulsante destro del mouse sulla cartella e scegliere il comando di menu **aggiungi&gt;nuovo test** . Verrà visualizzata la finestra di dialogo "Aggiungi nuovo test".

Si sceglie di creare uno "unit test" e denominarlo "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Quando si fa clic sul pulsante "OK", Visual Studio aggiungerà (e aprirà) un file DinnerTest.cs al progetto:

![](enable-automated-unit-testing/_static/image4.png)

Il modello di unit test di Visual Studio predefinito include una serie di codice della piastra calda che trovo un po' confuso. È sufficiente pulirlo per contenere solo il codice seguente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

L'attributo [TestClass] della classe DinnerTest precedente lo identifica come una classe che conterrà i test, nonché l'inizializzazione di test facoltativa e il codice teardown. È possibile definire i test al suo interno aggiungendo metodi pubblici con un attributo [TestMethod].

Di seguito sono riportati i primi due test che verranno aggiunti a questo esercizio della nostra classe dinner. Il primo test verifica che la cena non sia valida se viene creata una nuova cena senza che tutte le proprietà siano impostate correttamente. Il secondo test verifica che la cena sia valida quando una cena dispone di tutte le proprietà impostate con valori validi:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Si noterà che i nomi dei test sono molto espliciti (e poco dettagliati). Questa operazione viene stabilita perché si potrebbe creare centinaia o migliaia di test di piccole dimensioni e si vuole semplificare la determinazione rapida dello scopo e del comportamento di ognuno di essi (specialmente quando si esamina un elenco di errori in un test runner). I nomi dei test devono essere denominati dopo la funzionalità sottoposta a test. Sopra si usa un modello di denominazione "noun\_deve\_verbo".

Stiamo strutturando i test usando il modello di test "AAA", che sta per "Arrange, Act, Assert":

- Disponi: configura l'unità sottoposta a test
- Act: esercizio dell'unità sottoposta a test e acquisizione dei risultati
- Assert: verifica il comportamento

Quando si scrivono i test, si vuole evitare che i singoli test eseguano troppe operazioni. Ogni test deve invece verificare solo un singolo concetto (che renderà molto più semplice individuare la cause degli errori). Una guida efficace consiste nel provare a usare una sola istruzione Assert per ogni test. Se si dispone di più di un'istruzione Assert in un metodo di test, assicurarsi che siano tutti usati per testare lo stesso concetto. In caso di dubbi, effettuare un altro test.

### <a name="running-tests"></a>Esecuzione test

Visual Studio 2008 Professional (e versioni successive) include un Test Runner incorporato che può essere usato per eseguire i progetti di unit test di Visual Studio all'interno dell'IDE. Per eseguire tutti gli unit test, è possibile selezionare il comando di menu **Esegui test-&gt;&gt;tutti i test nel** menu della soluzione (o digitare CTRL R, a). In alternativa, è possibile posizionare il cursore all'interno di una classe di test o di un metodo di test specifico e usare il test **-&gt;esecuzione di test&gt;** il comando del menu di scelta rapida corrente (oppure premere CTRL R, t) per eseguire un subset degli unit test.

Posizionare il cursore all'interno della classe DinnerTest e digitare "CTRL R, T" per eseguire i due test appena definiti. Quando si esegue questa operazione, verrà visualizzata una finestra "Risultati test" in Visual Studio per visualizzare i risultati dell'esecuzione dei test elencati al suo interno:

![](enable-automated-unit-testing/_static/image5.png)

*Nota: per impostazione predefinita, nella finestra Risultati test di Visual Studio non viene visualizzata la colonna nome classe. È possibile aggiungere questa opzione facendo clic con il pulsante destro del mouse all'interno della finestra Risultati test e utilizzando il comando di menu Aggiungi/Rimuovi colonne.*

I due test hanno impiegato solo una frazione di un secondo per l'esecuzione, come si può notare che entrambi sono stati superati. È ora possibile aumentare le prestazioni creando test aggiuntivi che verificano convalide specifiche delle regole, oltre a coprire i due metodi helper, ovvero IsUserHost () e IsUserRegistered (), aggiunti alla classe dinner. La creazione di tutti questi test per la classe Dinner renderà molto più semplice e sicura l'aggiunta di nuove regole business e convalide in futuro. Possiamo aggiungere la nuova logica di regola a dinner, quindi, entro pochi secondi, verificare che non sia stata interruppe alcuna funzionalità della logica precedente.

Si noti che l'uso di un nome di test descrittivo rende più semplice comprendere rapidamente il risultato verificato da ogni test. Si consiglia di utilizzare il comando di menu **strumenti-&gt;opzioni** , aprire la schermata strumenti di test-&gt;configurazione esecuzione test e selezionare la casella di controllo "fare doppio clic su un risultato non riuscito o senza risultati unit test Visualizza il punto di errore nella casella di controllo test". In questo modo sarà possibile fare doppio clic su un errore nella finestra Risultati test e passare immediatamente all'errore di asserzione.

### <a name="creating-dinnerscontroller-unit-tests"></a>Creazione di unit test DinnersController

A questo punto, è possibile creare alcuni unit test per verificare la funzionalità DinnersController. Per iniziare, fare clic con il pulsante destro del mouse sulla cartella "Controllers" nel progetto di test, quindi scegliere il comando di menu **aggiungi&gt;nuovo test** . Creeremo uno "unit test" e denominarlo "DinnersControllerTest.cs".

Verranno creati due metodi di test che verificano il metodo di azione Details () su DinnersController. Il primo verificherà che venga restituita una vista quando viene richiesta una cena esistente. Il secondo verificherà che venga restituita una visualizzazione "NotFound" quando viene richiesta una cena non esistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Il codice precedente compila clean. Quando si eseguono i test, tuttavia, entrambi hanno esito negativo:

![](enable-automated-unit-testing/_static/image6.png)

Se si esaminano i messaggi di errore, si noterà che il motivo per cui i test hanno avuto esito negativo è dovuto al fatto che la classe DinnersRepository non è stata in grado di connettersi a un database. L'applicazione NerdDinner usa una stringa di connessione a un file di SQL Server Express locale che risiede nella directory \app\_data del progetto di applicazione NerdDinner. Poiché il progetto NerdDinner. tests viene compilato ed eseguito in una directory diversa, il progetto dell'applicazione, il percorso relativo della stringa di connessione non è corretto.

È *possibile* risolvere il problema copiando il file di database di SQL Express nel progetto di test e quindi aggiungere una stringa di connessione di test appropriata al progetto di test. Questa operazione otterrebbe i test sopra sbloccati e in esecuzione.

Il testing unità di codice con un database reale, tuttavia, comporta una serie di problemi. In particolare:

- Rallenta significativamente il tempo di esecuzione degli unit test. Maggiore è il tempo necessario per eseguire i test, minore è la probabilità di eseguirli di frequente. Idealmente si vuole che gli unit test siano in grado di essere eseguiti in pochi secondi e che sia un elemento che viene eseguito in modo naturale come la compilazione del progetto.
- Complica la logica di installazione e pulizia all'interno dei test. Si vuole che ogni unit test sia isolato e indipendente da altri, senza effetti collaterali o dipendenze. Quando si utilizza un database reale, è necessario essere consapevoli dello stato e reimpostarlo tra i test.

Esaminiamo uno schema progettuale denominato "inserimento delle dipendenze" che può aiutare a risolvere questi problemi ed evitare la necessità di usare un database reale con i nostri test.

### <a name="dependency-injection"></a>Inserimento di dipendenze

Attualmente DinnersController è strettamente "accoppiato" alla classe DinnerRepository. "Accoppiamento" si riferisce a una situazione in cui una classe si basa in modo esplicito su un'altra classe per funzionare:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Poiché la classe DinnerRepository richiede l'accesso a un database, la dipendenza strettamente associata della classe DinnersController sul DinnerRepository richiede di disporre di un database affinché i metodi di azione DinnersController siano testati.

Per aggirare questo problema, è possibile usare un modello di progettazione denominato "inserimento delle dipendenze", ovvero un approccio in cui le dipendenze (ad esempio le classi di repository che forniscono l'accesso ai dati) non vengono più create in modo implicito all'interno di classi che le usano. Al contrario, le dipendenze possono essere passate in modo esplicito alla classe che le utilizza usando gli argomenti del costruttore. Se le dipendenze vengono definite usando le interfacce, si ha la flessibilità di passare le implementazioni di dipendenza "Fake" per gli scenari unit test. In questo modo è possibile creare implementazioni di dipendenza specifiche del test che non richiedono effettivamente l'accesso a un database.

Per verificarlo, è necessario implementare l'inserimento delle dipendenze con la DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Estrazione di un'interfaccia IDinnerRepository

Il primo passaggio consiste nel creare una nuova interfaccia IDinnerRepository che incapsula il contratto di repository che i controller richiedono per recuperare e aggiornare le cene.

È possibile definire questo contratto di interfaccia manualmente facendo clic con il pulsante destro del mouse sulla cartella \Models e scegliendo il comando di menu **aggiungi&gt;nuovo elemento** e creando una nuova interfaccia denominata IDinnerRepository.cs.

In alternativa, è possibile usare gli strumenti di refactoring predefiniti Visual Studio Professional (e versioni successive) per estrarre e creare automaticamente un'interfaccia dalla classe DinnerRepository esistente. Per estrarre questa interfaccia con Visual Studio, posizionare semplicemente il cursore nell'editor di testo nella classe DinnerRepository, quindi fare clic con il pulsante destro del mouse e scegliere il comando di menu **refactoring-&gt;Estrai interfaccia** :

![](enable-automated-unit-testing/_static/image7.png)

Verrà avviata la finestra di dialogo "Estrai interfaccia" e verrà richiesto il nome dell'interfaccia da creare. L'impostazione predefinita è IDinnerRepository e seleziona automaticamente tutti i metodi pubblici sulla classe DinnerRepository esistente da aggiungere all'interfaccia:

![](enable-automated-unit-testing/_static/image8.png)

Quando si fa clic sul pulsante "OK", in Visual Studio viene aggiunta una nuova interfaccia IDinnerRepository all'applicazione:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

E la classe DinnerRepository esistente verrà aggiornata in modo che implementi l'interfaccia:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aggiornamento di DinnersController per supportare l'inserimento del costruttore

Verrà ora aggiornata la classe DinnersController per usare la nuova interfaccia.

Attualmente DinnersController è hardcoded in modo tale che il campo "dinnerRepository" sia sempre una classe DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Verrà modificato in modo che il campo "dinnerRepository" sia di tipo IDinnerRepository anziché DinnerRepository. Verranno quindi aggiunti due costruttori DinnersController pubblici. Uno dei costruttori consente il passaggio di un IDinnerRepository come argomento. L'altro è un costruttore predefinito che usa l'implementazione di DinnerRepository esistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Poiché per impostazione predefinita MVC ASP.NET crea classi controller usando costruttori predefiniti, il DinnersController in fase di esecuzione continuerà a usare la classe DinnerRepository per eseguire l'accesso ai dati.

È ora possibile aggiornare gli unit test per passare un'implementazione "Fake" del repository Dinner usando il costruttore di parametri. Questo repository di cene "Fake" non richiede l'accesso a un database reale e usa invece i dati di esempio in memoria.

#### <a name="creating-the-fakedinnerrepository-class"></a>Creazione della classe FakeDinnerRepository

Viene ora creata una classe FakeDinnerRepository.

Si inizierà con la creazione di una directory "Fakes" all'interno del progetto NerdDinner. tests, quindi si aggiungerà una nuova classe FakeDinnerRepository (fare clic con il pulsante destro del mouse sulla cartella e scegliere **Add-&gt;nuova classe**):

![](enable-automated-unit-testing/_static/image9.png)

Il codice verrà aggiornato in modo che la classe FakeDinnerRepository implementi l'interfaccia IDinnerRepository. È possibile fare clic con il pulsante destro del mouse su di esso e scegliere il comando del menu di scelta rapida "implementa interfaccia IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

In questo modo Visual Studio aggiungerà automaticamente tutti i membri dell'interfaccia IDinnerRepository alla classe FakeDinnerRepository con implementazioni predefinite di "Stub out":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

È quindi possibile aggiornare l'implementazione di FakeDinnerRepository in modo che funzioni da un elenco in memoria&lt;Dinner&gt; Collection passato come argomento del costruttore:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

È ora disponibile un'implementazione di IDinnerRepository fittizia che non richiede un database e può invece usare un elenco in memoria di oggetti dinner.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Uso di FakeDinnerRepository con unit test

Torniamo agli unit test DinnersController che hanno avuto esito negativo in precedenza, perché il database non era disponibile. È possibile aggiornare i metodi di test per usare un FakeDinnerRepository popolato con i dati di cena in memoria di esempio in DinnersController usando il codice seguente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

A questo punto, quando si eseguono questi test, entrambi passano:

![](enable-automated-unit-testing/_static/image11.png)

Meglio, richiedono solo una frazione di un secondo per l'esecuzione e non richiedono alcuna logica di installazione/pulizia complessa. È ora possibile unit test tutto il codice del metodo di azione DinnersController (inclusi elenchi, paging, dettagli, creazione, aggiornamento ed eliminazione) senza mai dover connettersi a un database reale.

| **Argomento laterale: Framework di inserimento delle dipendenze** |
| --- |
| L'inserimento manuale delle dipendenze (come sopra) funziona correttamente, ma diventa più difficile da gestire quando aumenta il numero di dipendenze e componenti in un'applicazione. Sono disponibili diversi framework di inserimento delle dipendenze per .NET che consentono di garantire una maggiore flessibilità di gestione delle dipendenze. Questi Framework, anche a volte detti contenitori IoC (Inversion of Control), forniscono meccanismi che consentono un ulteriore livello di supporto della configurazione per specificare e passare le dipendenze agli oggetti in fase di esecuzione (più spesso usando l'inserimento del costruttore ). Di seguito sono riportati alcuni dei più diffusi framework di inserimento/IOC delle dipendenze OSS in .NET: AutoFac, Ninject, Spring.NET, StructureMap e Windsor. ASP.NET MVC espone le API di estendibilità che consentono agli sviluppatori di partecipare alla risoluzione e alla creazione di istanze dei controller e che consente di integrare in questo processo i Framework di inserimento delle dipendenze/IoC. L'uso di un framework DI i/o IOC ci consente anche di rimuovere il costruttore predefinito dal DinnersController, che rimuove completamente l'accoppiamento tra l'oggetto e il DinnerRepository. Non verrà usato un Framework di inserimento delle dipendenze/IOC con l'applicazione NerdDinner. Tuttavia, è possibile prendere in considerazione il futuro se la base di codice e le funzionalità di NerdDinner sono aumentate. |

### <a name="creating-edit-action-unit-tests"></a>Creazione di unit test azione di modifica

Verranno ora creati alcuni unit test che verificano la funzionalità di modifica di DinnersController. Si inizierà testando la versione HTTP-GET dell'azione di modifica:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Verrà creato un test che verifica che venga eseguito il rendering di una visualizzazione supportata da un oggetto DinnerFormViewModel quando viene richiesta una cena valida:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Quando si esegue il test, tuttavia, si noterà che l'operazione ha esito negativo perché viene generata un'eccezione di riferimento null quando il metodo Edit accede alla proprietà User.Identity.Name per eseguire il controllo dinner. IsHostedBy ().

L'oggetto utente nella classe di base controller incapsula i dettagli sull'utente connesso e viene popolato da ASP.NET MVC quando crea il controller in fase di esecuzione. Poiché si sta testando il DinnersController all'esterno di un ambiente server Web, l'oggetto utente non è impostato, di conseguenza l'eccezione di riferimento null.

### <a name="mocking-the-useridentityname-property"></a>Simulazione della proprietà User.Identity.Name

I Framework di simulazione semplificano i test consentendo di creare in modo dinamico versioni Fake di oggetti dipendenti che supportano i test. Ad esempio, è possibile usare un Framework fittizio nel test azione di modifica per creare dinamicamente un oggetto utente che il DinnersController può usare per cercare un nome utente simulato. In questo modo si eviterà che un riferimento null venga generato quando si esegue il test.

Sono disponibili molti Framework di simulazione .NET che possono essere usati con ASP.NET MVC (è possibile visualizzarne un elenco qui: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)). Per il test dell'applicazione NerdDinner verrà usato un Framework di simulazione open source denominato "MOQ", che può essere scaricato gratuitamente da [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).

Al termine del download, verrà aggiunto un riferimento nel progetto NerdDinner. tests all'assembly MOQ. dll:

![](enable-automated-unit-testing/_static/image12.png)

Si aggiungerà quindi un metodo helper "CreateDinnersControllerAs (username)" alla classe di test che accetta un nome utente come parametro e che quindi "simula" la proprietà User.Identity.Name nell'istanza di DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Sopra si usa MOQ per creare un oggetto fittizio che simula un oggetto ControllerContext (che ASP.NET MVC passa alle classi controller per esporre oggetti di runtime quali utente, richiesta, risposta e sessione). Viene chiamato il metodo "SetupGet" sulla simulazione per indicare che la proprietà HttpContext.User.Identity.Name in ControllerContext deve restituire la stringa username passata al metodo helper.

È possibile simulare un numero qualsiasi di proprietà e metodi di ControllerContext. Per illustrare questo problema, ho aggiunto anche una chiamata a SetupGet () per la proprietà Request. IsAuthenticated (che non è effettivamente necessaria per i test seguenti, ma che consente di illustrare come è possibile simulare le proprietà della richiesta). Al termine, viene assegnata un'istanza della simulazione ControllerContext al DinnersController il metodo di supporto viene restituito.

È ora possibile scrivere unit test che usano questo metodo helper per testare gli scenari di modifica che coinvolgono utenti diversi:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

A questo punto, quando si eseguono i test superati:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Test di scenari UpdateModel ()

Sono stati creati test che coprono la versione HTTP-GET dell'azione di modifica. Verranno ora creati alcuni test che verificano la versione HTTP-POST dell'azione di modifica:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Il nuovo scenario di test interessante da supportare con questo metodo di azione è l'utilizzo del metodo helper UpdateModel () nella classe di base del controller. Questo metodo helper viene usato per associare i valori dei post di form all'istanza dell'oggetto dinner.

Di seguito sono riportati due test che illustrano come è possibile fornire i valori inviati del modulo per il metodo helper UpdateModel () da usare. Per eseguire questa operazione, è necessario creare e popolare un oggetto FormCollection, quindi assegnarlo alla proprietà "ValueProvider" nel controller.

Il primo test verifica che in un salvataggio riuscito il browser venga reindirizzato all'azione dettagli. Il secondo test verifica che, quando viene pubblicato un input non valido, l'azione rivisualizzi nuovamente la visualizzazione di modifica con un messaggio di errore.

[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Test a capo

Sono stati analizzati i concetti di base relativi alle classi controller unit test. È possibile usare queste tecniche per creare con facilità centinaia di semplici test che verificano il comportamento dell'applicazione.

Poiché i test del controller e del modello non richiedono un database reale, sono estremamente veloci e facili da eseguire. Potrai eseguire centinaia di test automatizzati in pochi secondi e ricevere immediatamente commenti e suggerimenti sull'eventuale modifica apportata a un problema. In questo modo, ci consentirà di migliorare continuamente, effettuare il refactoring e affinare l'applicazione.

Il test è stato trattato come ultimo argomento di questo capitolo, ma non perché i test sono elementi da eseguire al termine di un processo di sviluppo. Al contrario, è consigliabile scrivere test automatizzati il prima possibile nel processo di sviluppo. In questo modo è possibile ottenere un feedback immediato durante lo sviluppo, che consente di riflettere attentamente sugli scenari dei casi d'uso dell'applicazione e consente di progettare l'applicazione con livelli e accoppiamento puliti.

Un capitolo successivo del libro illustrerà lo sviluppo basato su test (TDD) e il modo in cui usarlo con ASP.NET MVC. TDD è una procedura di codifica iterativa in cui è necessario prima scrivere i test che il codice risultante soddisferà. Con TDD si inizia ogni funzionalità creando un test per verificare la funzionalità che si sta per implementare. Scrivere il unit test per prima cosa consente di comprendere chiaramente la funzionalità e come dovrebbe funzionare. Quando il test è stato scritto e si è verificato un errore, è necessario implementare la funzionalità effettiva verificata dal test. Poiché si è già trascorso tempo pensando al caso d'uso di come la funzionalità dovrebbe funzionare, si avrà una migliore comprensione dei requisiti e del modo migliore per implementarli. Al termine dell'implementazione, è possibile eseguire nuovamente il test e ottenere un feedback immediato per verificare se la funzionalità funziona correttamente. Nel capitolo 10 verrà trattata una maggiore quantità di informazioni su TDD.

### <a name="next-step"></a>Passo successivo

Alcuni commenti finali di riepilogo.

> [!div class="step-by-step"]
> [Precedente](use-ajax-to-implement-mapping-scenarios.md)
> [Successivo](nerddinner-wrap-up.md)
