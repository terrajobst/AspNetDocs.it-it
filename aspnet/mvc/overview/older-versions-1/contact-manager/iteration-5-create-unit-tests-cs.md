---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
title: '#5 iterazione – creare unit testC#() | Microsoft Docs'
author: microsoft
description: Nella quinta iterazione, l'applicazione viene semplificata per la manutenzione e la modifica mediante l'aggiunta di unit test. Si simulano le classi del modello di dati e si compilano unit test per o...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 28ad8f80-b8a5-444e-b478-8b15a846060c
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
msc.type: authoredcontent
ms.openlocfilehash: 32e81cce34a0e0b1f6b01934334e1b66dce89651
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544303"
---
# <a name="iteration-5--create-unit-tests-c"></a>#5 iterazione – creare unit testC#()

[Microsoft](https://github.com/microsoft)

[Scarica codice](iteration-5-create-unit-tests-cs/_static/contactmanager_5_cs1.zip)

> Nella quinta iterazione, l'applicazione viene semplificata per la manutenzione e la modifica mediante l'aggiunta di unit test. Si simulano le classi del modello di dati e si compilano unit test per i controller e la logica di convalida.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Compilazione di un'applicazione MVC di gestione deiC#contatti (ASP.NET)

In questa serie di esercitazioni viene creata un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di archiviare i nomi delle informazioni di contatto, i numeri di telefono e gli indirizzi di posta elettronica per un elenco di persone.

L'applicazione viene compilata su più iterazioni. Ogni iterazione migliora gradualmente l'applicazione. L'obiettivo di questo approccio di iterazione multipla è quello di consentire di comprendere il motivo di ogni modifica.

- #1 iterazione: creare l'applicazione. Nella prima iterazione, il gestore contatti viene creato nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base sul database: creazione, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2: rendere l'applicazione un aspetto gradevole. In questa iterazione, si migliora l'aspetto dell'applicazione modificando la pagina master e il foglio di stile CSS predefiniti della visualizzazione MVC ASP.NET.

- #3 iterazione: aggiungere la convalida del modulo. Nella terza iterazione viene aggiunta la convalida dei form di base. Si impedisce agli utenti di inviare un modulo senza completare i campi dei moduli richiesti. Vengono convalidati anche gli indirizzi di posta elettronica e i numeri di telefono.

- Iterazione #4: rendere l'applicazione a regime di controllo libero. In questa quarta iterazione sono disponibili diversi modelli di progettazione software che semplificano la gestione e la modifica dell'applicazione Contact Manager. Ad esempio, si effettua il refactoring dell'applicazione per usare il modello di repository e il modello di inserimento delle dipendenze.

- Iterazione #5-creare unit test. Nella quinta iterazione, l'applicazione viene semplificata per la manutenzione e la modifica mediante l'aggiunta di unit test. Si simulano le classi del modello di dati e si compilano unit test per i controller e la logica di convalida.

- #6 iterazione: usare lo sviluppo basato su test. In questa sesta iterazione vengono aggiunte nuove funzionalità all'applicazione scrivendo unit test prima e scrivendo il codice in base agli unit test. In questa iterazione vengono aggiunti gruppi di contatti.

- #7 iterazione: aggiungere la funzionalità AJAX. Nella settima iterazione, si migliora la velocità di risposta e le prestazioni dell'applicazione mediante l'aggiunta del supporto per AJAX.

## <a name="this-iteration"></a>Questa iterazione

Nell'iterazione precedente dell'applicazione Contact Manager è stato eseguito il refactoring dell'applicazione in modo da essere più a regime di controllo libero. L'applicazione è stata separata in livelli di controller, servizio e repository distinti. Ogni livello interagisce con il livello sottostante attraverso le interfacce.

L'applicazione è stata sottoposta a refactoring per semplificare la manutenzione e la modifica dell'applicazione. Se, ad esempio, è necessario usare una nuova tecnologia di accesso ai dati, è possibile modificare semplicemente il livello del repository senza toccare il controller o il livello di servizio. Rendendo il gestore dei contatti a regime di controllo libero, l'applicazione è più resiliente per la modifica.

Ma cosa accade quando è necessario aggiungere una nuova funzionalità all'applicazione Contact Manager? Oppure, cosa accade quando si corregge un bug? Una triste, ma ben collaudata, la verità della scrittura di codice è che ogni volta che si tocca il codice si crea il rischio di introdurre nuovi bug.

Ad esempio, un giorno fine, il responsabile potrebbe chiedere di aggiungere una nuova funzionalità al gestore contatti. Desidera aggiungere il supporto per i gruppi di contatti. Desidera consentire agli utenti di organizzare i contatti in gruppi, ad esempio amici, aziende e così via.

Per implementare questa nuova funzionalità, è necessario modificare tutti e tre i livelli dell'applicazione Contact Manager. È necessario aggiungere nuove funzionalità ai controller, al livello di servizio e al repository. Non appena si inizia a modificare il codice, si rischia di suddividere le funzionalità che hanno funzionato in precedenza.

Il refactoring dell'applicazione in livelli distinti, come nell'iterazione precedente, era un aspetto positivo. Si tratta di un aspetto positivo perché consente di apportare modifiche a tutti i livelli senza toccare il resto dell'applicazione. Tuttavia, se si desidera rendere il codice all'interno di un livello più semplice da gestire e modificare, è necessario creare unit test per il codice.

Usare un unit test per testare una singola unità di codice. Queste unità di codice sono inferiori a tutti i livelli dell'applicazione. In genere si usa un unit test per verificare se un particolare metodo nel codice si comporta nel modo previsto. È ad esempio possibile creare un unit test per il metodo CreateContact () esposto dalla classe ContactManagerService.

Gli unit test per un'applicazione funzionano esattamente come una rete di protezione. Ogni volta che si modifica il codice in un'applicazione, è possibile eseguire un set di unit test per verificare se la modifica interrompe la funzionalità esistente. Gli unit test rendono il codice sicuro per la modifica. Gli unit test rendono tutto il codice nell'applicazione più resiliente per la modifica.

In questa iterazione gli unit test vengono aggiunti all'applicazione Contact Manager. In questo modo, nell'iterazione successiva, è possibile aggiungere gruppi di contatti all'applicazione senza doversi preoccupare delle interruzioni della funzionalità esistente.

> [!NOTE] 
> 
> Sono disponibili diversi framework per unit test, tra cui NUnit, xUnit.net e MbUnit. In questa esercitazione viene usato il Framework di unit test incluso in Visual Studio. Tuttavia, è possibile usare uno di questi framework alternativi in modo semplice.

## <a name="what-gets-tested"></a>Cosa viene testato

Nel mondo perfetto tutto il codice verrebbe analizzato dagli unit test. Nel mondo perfetto, si otterrebbe la rete di sicurezza perfetta. Sarà possibile modificare qualsiasi riga di codice nell'applicazione e sapere immediatamente, eseguendo gli unit test, se la modifica ha interrotto la funzionalità esistente.

Tuttavia, non viviamo in un mondo perfetto. In pratica, quando si scrivono unit test, è possibile concentrarsi sulla scrittura di test per la logica di business (ad esempio, la logica di convalida). In particolare, *non* si scrivono unit test per la logica di accesso ai dati o la logica di visualizzazione.

Per essere utile, gli unit test devono essere eseguiti molto rapidamente. È possibile accumulare facilmente centinaia o addirittura migliaia di unit test per un'applicazione. Se l'esecuzione degli unit test richiede molto tempo, si eviterà di eseguirli. In altre parole, gli unit test a esecuzione prolungata sono inutili per scopi di codifica giornalieri.

Per questo motivo, in genere non si scrivono unit test per il codice che interagisce con un database. L'esecuzione di centinaia di unit test in un database attivo è troppo lenta. Al contrario, si simula il database e si scrive il codice che interagisce con il database fittizio (si tratta di una simulazione di un database riportato di seguito).

Per un motivo analogo, in genere non si scrivono unit test per le visualizzazioni. Per testare una visualizzazione, è necessario creare un server Web. Poiché la rotazione di un server Web è un processo relativamente lento, non è consigliabile creare unit test per le visualizzazioni.

Se la vista contiene una logica complessa, è necessario prendere in considerazione lo stato di trasferimento della logica nei metodi helper. È possibile scrivere unit test per metodi helper che vengono eseguiti senza avviare un server Web.

> [!NOTE] 
> 
> Sebbene la scrittura di test per la logica di accesso ai dati o la logica di visualizzazione non sia una valida idea quando si scrivono unit test, questi test possono essere molto utili quando si creano test funzionali o di integrazione.

> [!NOTE] 
> 
> ASP.NET MVC è il motore di visualizzazione Web Form. Sebbene il motore di visualizzazione Web Form dipenda da un server Web, altri motori di visualizzazione potrebbero non essere.

## <a name="using-a-mock-object-framework"></a>Uso di un Framework di oggetti fittizio

Quando si compilano unit test, è quasi sempre necessario sfruttare un Framework di oggetti fittizio. Un Framework di oggetti fittizio consente di creare simulazioni e stub per le classi nell'applicazione.

Ad esempio, è possibile usare un Framework di oggetti fittizio per generare una versione fittizia della classe del repository. In questo modo, è possibile usare la classe del repository fittizio invece della classe del repository reale negli unit test. L'utilizzo del repository fittizio consente di evitare l'esecuzione di codice del database durante l'esecuzione di un unit test.

Visual Studio non include un Framework di oggetti fittizio. Per .NET Framework sono tuttavia disponibili diversi framework di oggetti fittizi commerciali e Open Source:

1. MOQ: questo Framework è disponibile con la licenza open source BSD. È possibile scaricare MOQ da [https://code.google.com/p/moq/](https://code.google.com/p/moq/).
2. Rhino Mocks: questo Framework è disponibile con la licenza open source BSD. È possibile scaricare Rhino Mocks da [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).
3. Isolatore Typemock-si tratta di un Framework commerciale. È possibile scaricare una versione di valutazione da [http://www.typemock.com/](http://www.typemock.com/).

In questa esercitazione ho deciso di usare MOQ. Tuttavia, è possibile usare facilmente Rhino Mocks o Typemock Isolator per creare gli oggetti fittizi per l'applicazione Contact Manager.

Prima di poter usare MOQ, è necessario completare i passaggi seguenti:

1. .
2. Prima di decomprimere il download, assicurarsi di fare clic con il pulsante destro del mouse sul file e fare clic sul pulsante **Sblocca** (vedere la figura 1).
3. Decomprimere il download.
4. Aggiungere un riferimento all'assembly MOQ facendo clic con il pulsante destro del mouse sulla cartella riferimenti nel progetto ContactManager. tests e scegliendo **Aggiungi riferimento**. Nella scheda Sfoglia passare alla cartella in cui è stato decompresso MOQ e selezionare l'assembly MOQ. dll. Fare clic sul pulsante **OK** .
5. Dopo aver completato questi passaggi, la cartella References avrà un aspetto simile a quello della figura 2.

[![di sblocco MOQ](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)

**Figura 01**. sblocco di MOQ ([fare clic per visualizzare un'immagine a dimensione intera](iteration-5-create-unit-tests-cs/_static/image2.png))

[![riferimenti dopo l'aggiunta di MOQ](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)

**Figura 02**: riferimenti dopo l'aggiunta di MOQ ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-5-create-unit-tests-cs/_static/image4.png))

## <a name="creating-unit-tests-for-the-service-layer"></a>Creazione di unit test per il livello di servizio

Inizia creando un set di unit test per il livello di servizio dell'applicazione Contact Manager. Questi test verranno usati per verificare la logica di convalida.

Creare una nuova cartella denominata Models nel progetto ContactManager. tests. Fare quindi clic con il pulsante destro del mouse sulla cartella Models e scegliere **Aggiungi, nuovo test**. Viene visualizzata la finestra di dialogo **Aggiungi nuovo test** visualizzata nella figura 3. Selezionare il modello di **unit test** e denominare il nuovo test ContactManagerServiceTest.cs. Fare clic sul pulsante **OK** per aggiungere il nuovo test al progetto di test.

> [!NOTE] 
> 
> In generale, si vuole che la struttura di cartelle del progetto di test corrisponda alla struttura di cartelle del progetto MVC ASP.NET. Ad esempio, si inseriscono i test del controller in una cartella dei controller, si modellano i test in una cartella Models e così via.

[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)

**Figura 03**: Models\ContactManagerServiceTest.cs ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-5-create-unit-tests-cs/_static/image6.png))

Inizialmente si vuole testare il metodo CreateContact () esposto dalla classe ContactManagerService. Verranno creati i cinque test seguenti:

- CreateContact (): verifica che CreateContact () restituisca il valore true quando un contatto valido viene passato al metodo.
- CreateContactRequiredFirstName (): verifica che un messaggio di errore venga aggiunto allo stato del modello quando un contatto con un nome mancante viene passato al metodo CreateContact ().
- CreateContactRequiredLastName (): verifica che un messaggio di errore venga aggiunto allo stato del modello quando viene passato un contatto con un cognome mancante al metodo CreateContact ().
- CreateContactInvalidPhone (): verifica che un messaggio di errore venga aggiunto allo stato del modello quando un contatto con un numero di telefono non valido viene passato al metodo CreateContact ().
- CreateContactInvalidEmail (): verifica che un messaggio di errore venga aggiunto allo stato del modello quando viene passato un contatto con un indirizzo di posta elettronica non valido al metodo CreateContact ().

Il primo test verifica che un contatto valido non generi un errore di convalida. I test rimanenti controllano ogni regola di convalida.

Il codice per questi test è contenuto nel listato 1.

**Listato 1-Models\ContactManagerServiceTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample1.cs)]

Poiché si usa la classe Contact nel listato 1, è necessario aggiungere un riferimento a Microsoft Entity Framework al progetto di test. Aggiungere un riferimento all'assembly System. Data. Entity.

Il listato 1 contiene un metodo denominato Initialize () decorato con l'attributo [TestInitialize]. Questo metodo viene chiamato automaticamente prima dell'esecuzione di ciascuno degli unit test (viene chiamato 5 volte prima di ciascuno degli unit test). Il metodo Initialize () crea un repository fittizio con la riga di codice seguente:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample2.cs)]

Questa riga di codice usa il Framework MOQ per generare un repository fittizio dall'interfaccia IContactManagerRepository. Il repository fittizio viene usato al posto del EntityContactManagerRepository effettivo per evitare l'accesso al database quando ogni unit test viene eseguito. Il repository fittizio implementa i metodi dell'interfaccia IContactManagerRepository, ma i metodi non eseguono alcuna operazione.

> [!NOTE] 
> 
> Quando si usa il Framework MOQ, esiste una distinzione tra \_mockRepository e \_mockRepository. Object. Il primo si riferisce alla classe di&gt; IContactManagerRepository di simulazione&lt;che contiene i metodi per specificare come si comporterà il repository fittizio. Quest'ultimo si riferisce al repository fittizio effettivo che implementa l'interfaccia IContactManagerRepository.

Il repository fittizio viene usato nel metodo Initialize () quando si crea un'istanza della classe ContactManagerService. Tutti i singoli unit test utilizzano questa istanza della classe ContactManagerService.

L'elenco 1 contiene cinque metodi che corrispondono a ognuno degli unit test. Ognuno di questi metodi è decorato con l'attributo [TestMethod]. Quando si eseguono gli unit test, viene chiamato qualsiasi metodo con questo attributo. In altre parole, qualsiasi metodo decorato con l'attributo [TestMethod] è un unit test.

Il primo unit test, denominato CreateContact (), verifica che la chiamata di CreateContact () restituisca il valore true quando un'istanza valida della classe Contact viene passata al metodo. Il test crea un'istanza della classe Contact, chiama il metodo CreateContact () e verifica che CreateContact () restituisca il valore true.

I test rimanenti verificano che quando viene chiamato il metodo CreateContact () con un contatto non valido, il metodo restituisce false e il messaggio di errore di convalida previsto viene aggiunto allo stato del modello. Ad esempio, il test CreateContactRequiredFirstName () crea un'istanza della classe Contact con una stringa vuota per la relativa proprietà FirstName. Successivamente, il metodo CreateContact () viene chiamato con il contatto non valido. Infine, il test verifica che CreateContact () restituisca false e che lo stato del modello contenga il messaggio di errore di convalida previsto "il nome è obbligatorio".

È possibile eseguire gli unit test nel listato 1 selezionando l'opzione di menu **test, Esegui, tutti i test nella soluzione (CTRL + R, a)** . I risultati dei test vengono visualizzati nella finestra di Risultati test (vedere la figura 4).

[![Risultati test](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)

**Figura 04**: risultati test ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-5-create-unit-tests-cs/_static/image8.png))

## <a name="creating-unit-tests-for-controllers"></a>Creazione di unit test per i controller

L'applicazione ASP. NETMVC controlla il flusso di interazione dell'utente. Quando si esegue il test di un controller, si desidera verificare se il controller restituisce il risultato dell'azione corretta e visualizzare i dati. Potrebbe inoltre essere necessario verificare se un controller interagisce con le classi del modello nel modo previsto.

Ad esempio, l'elenco 2 contiene due unit test per il metodo Contact controller Create (). Il primo unit test verifica che, quando un contatto valido viene passato al metodo Create (), il metodo Create () reindirizza all'azione index. In altre parole, quando viene passato un contatto valido, il metodo Create () deve restituire un RedirectToRouteResult che rappresenta l'azione index.

Non si vuole testare il livello del servizio ContactManager quando si sta testando il livello del controller. Si simula quindi il livello di servizio con il codice seguente nel metodo Initialize:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample3.cs)]

In CreateValidContact () unit test si simula il comportamento della chiamata al metodo CreateContact () del livello di servizio con la riga di codice seguente:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample4.cs)]

Questa riga di codice causa la restituzione del valore true da parte del servizio ContactManager fittizio quando viene chiamato il metodo CreateContact (). Simulando il livello di servizio, è possibile testare il comportamento del controller senza dover eseguire codice nel livello di servizio.

Il secondo unit test verifica che l'azione Create () restituisca la vista create quando al metodo viene passato un contatto non valido. Si fa in modo che il metodo CreateContact () del livello di servizio restituisca il valore false con la riga di codice seguente:

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample5.cs)]

Se il metodo Create () si comporta come previsto, deve restituire la vista create quando il livello di servizio restituisce il valore false. In questo modo, il controller può visualizzare i messaggi di errore di convalida nella visualizzazione create e l'utente ha la possibilità di correggere le proprietà del contatto non valido.

Se si prevede di creare unit test per i controller, è necessario restituire nomi di visualizzazione espliciti dalle azioni del controller. Ad esempio, non restituire una visualizzazione simile alla seguente:

Visualizzazione restituita ();

Al contrario, restituire la visualizzazione come segue:

Visualizzazione restituita ("create");

Se non si è espliciti quando si restituisce una visualizzazione, la proprietà ViewResult. ViewName restituisce una stringa vuota.

**Listato 2-Controllers\ContactControllerTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample6.cs)]

## <a name="summary"></a>Riepilogo

In questa iterazione sono stati creati unit test per l'applicazione Contact Manager. Possiamo eseguire questi unit test in qualsiasi momento per verificare che l'applicazione continui a funzionare nel modo previsto. Gli unit test fungono da rete di sicurezza per l'applicazione che ci permette di modificare in modo sicuro l'applicazione in futuro.

Sono stati creati due set di unit test. Per prima cosa, è stata testata la logica di convalida creando unit test per il livello di servizio. Successivamente, è stata testata la logica di controllo di flusso creando unit test per il livello controller. Quando si esegue il test del livello di servizio, i test per il livello di servizio sono stati isolati dal livello di repository, simulando il livello del repository. Quando si esegue il test del livello controller, i test per il livello controller sono stati isolati simulando il livello di servizio.

Nell'iterazione successiva verrà modificata l'applicazione Contact Manager in modo che supporti i gruppi di contatti. Questa nuova funzionalità verrà aggiunta all'applicazione tramite un processo di progettazione software denominato sviluppo basato su test.

> [!div class="step-by-step"]
> [Precedente](iteration-4-make-the-application-loosely-coupled-cs.md)
> [Successivo](iteration-6-use-test-driven-development-cs.md)
