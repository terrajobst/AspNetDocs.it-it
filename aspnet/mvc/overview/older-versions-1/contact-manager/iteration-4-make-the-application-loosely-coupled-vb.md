---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: "#4 iterazione: rendere l'applicazione a regime di controllo libero (VB) | Microsoft Docs"
author: microsoft
description: In questa quarta iterazione sono disponibili diversi modelli di progettazione software che semplificano la gestione e la modifica dell'applicazione Contact Manager. Per...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 422c75406d9c08279d0c2224ee4b6db3a71eb1b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582082"
---
# <a name="iteration-4--make-the-application-loosely-coupled-vb"></a>#4 iterazione: rendere l'applicazione a regime di controllo libero (VB)

[Microsoft](https://github.com/microsoft)

[Scarica codice](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> In questa quarta iterazione sono disponibili diversi modelli di progettazione software che semplificano la gestione e la modifica dell'applicazione Contact Manager. Ad esempio, si effettua il refactoring dell'applicazione per usare il modello di repository e il modello di inserimento delle dipendenze.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Compilazione di un'applicazione MVC di gestione dei contatti ASP.NET (VB)

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

In questa quarta iterazione dell'applicazione Contact Manager, viene fatto il refactoring dell'applicazione in modo da rendere l'applicazione più a regime di controllo libero. Quando un'applicazione è a regime di controllo libero, è possibile modificare il codice in una parte dell'applicazione senza dover modificare il codice in altre parti dell'applicazione. Le applicazioni a regime di controllo libero sono più resilienti per la modifica.

Attualmente, tutte le logiche di convalida e accesso ai dati utilizzate dall'applicazione Contact Manager sono contenute nelle classi controller. Si tratta di una pessima idea. Ogni volta che è necessario modificare una parte dell'applicazione, si rischia di introdurre bug in un'altra parte dell'applicazione. Se, ad esempio, si modifica la logica di convalida, si rischia di introdurre nuovi bug nell'accesso ai dati o nella logica del controller.

> [!NOTE] 
> 
> (SRP), una classe non deve mai avere più di un motivo per la modifica. La combinazione di controller, convalida e logica del database è una violazione massiccia del principio di singola responsabilità.

Esistono diversi motivi per cui potrebbe essere necessario modificare l'applicazione. Potrebbe essere necessario aggiungere una nuova funzionalità all'applicazione, potrebbe essere necessario correggere un bug nell'applicazione oppure potrebbe essere necessario modificare il modo in cui viene implementata una funzionalità dell'applicazione. Le applicazioni sono raramente statiche. Tendono a crescere e a mutare nel tempo.

Si supponga, ad esempio, che si decida di modificare la modalità di implementazione del livello di accesso ai dati. A questo punto, l'applicazione Contact Manager usa il Entity Framework Microsoft per accedere al database. Tuttavia, è possibile decidere di eseguire la migrazione a una tecnologia di accesso ai dati nuova o alternativa, ad esempio ADO.NET Data Services o NHibernate. Tuttavia, poiché il codice di accesso ai dati non è isolato dal codice di convalida e del controller, non è possibile modificare il codice di accesso ai dati nell'applicazione senza modificare altro codice non direttamente correlato all'accesso ai dati.

Quando un'applicazione è a regime di controllo libero, d'altra parte, è possibile apportare modifiche a una parte di un'applicazione senza toccare altre parti di un'applicazione. Ad esempio, è possibile cambiare le tecnologie di accesso ai dati senza modificare la logica del controller o della convalida.

In questa iterazione si approfitteranno dei diversi modelli di progettazione software che consentono di effettuare il refactoring dell'applicazione Contact Manager in un'applicazione più a regime di controllo libero. Al termine, il responsabile del contatto ha vinto qualsiasi operazione non eseguita in precedenza. Tuttavia, in futuro sarà possibile modificare l'applicazione in modo più semplice.

> [!NOTE] 
> 
> Il refactoring è il processo di riscrittura di un'applicazione in modo che non perda alcuna funzionalità esistente.

## <a name="using-the-repository-software-design-pattern"></a>Uso del modello di progettazione software del repository

La prima modifica consiste nel trarre vantaggio da un modello di progettazione software denominato modello di repository. Il modello di repository verrà usato per isolare il codice di accesso ai dati dal resto dell'applicazione.

Per implementare il modello di repository, è necessario completare i due passaggi seguenti:

1. Creare un'interfaccia
2. Creare una classe concreta che implementi l'interfaccia

In primo luogo, è necessario creare un'interfaccia che descriva tutti i metodi di accesso ai dati che è necessario eseguire. L'interfaccia IContactManagerRepository è inclusa nel listato 1. Questa interfaccia descrive cinque metodi: CreateContact (), DeleteContact (), EditContact (), GetContact e ListContacts ().

**Listato 1-Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

Successivamente, è necessario creare una classe concreta che implementi l'interfaccia IContactManagerRepository. Poiché si sta utilizzando Microsoft Entity Framework per accedere al database, verrà creata una nuova classe denominata EntityContactManagerRepository. Questa classe è inclusa nel listato 2.

**Listato 2-Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Si noti che la classe EntityContactManagerRepository implementa l'interfaccia IContactManagerRepository. La classe implementa tutti i cinque metodi descritti da tale interfaccia.

Ci si potrebbe chiedere perché è necessario preoccuparsi di un'interfaccia. Perché è necessario creare un'interfaccia e una classe che la implementi?

Con un'unica eccezione, il resto dell'applicazione interagisce con l'interfaccia e non con la classe concreta. Anziché chiamare i metodi esposti dalla classe EntityContactManagerRepository, verranno chiamati i metodi esposti dall'interfaccia IContactManagerRepository.

In questo modo, è possibile implementare l'interfaccia con una nuova classe senza dover modificare il resto dell'applicazione. Ad esempio, in una data futura, potrebbe essere necessario implementare una classe DataServicesContactManagerRepository che implementi l'interfaccia IContactManagerRepository. La classe DataServicesContactManagerRepository può utilizzare ADO.NET Data Services per accedere a un database anziché a Microsoft Entity Framework.

Se il codice dell'applicazione viene programmato con l'interfaccia IContactManagerRepository anziché con la classe EntityContactManagerRepository concreta, è possibile passare a classi concrete senza modificare il resto del codice. Ad esempio, è possibile passare dalla classe EntityContactManagerRepository alla classe DataServicesContactManagerRepository senza modificare la logica di convalida o l'accesso ai dati.

La programmazione in base alle interfacce (astrazioni) anziché a classi concrete rende più resiliente la modifica dell'applicazione.

> [!NOTE] 
> 
> È possibile creare rapidamente un'interfaccia da una classe concreta in Visual Studio selezionando l'opzione di menu Refactoring, Estrai interfaccia. Ad esempio, è possibile creare prima la classe EntityContactManagerRepository e quindi usare Estrai interfaccia per generare automaticamente l'interfaccia IContactManagerRepository.

## <a name="using-the-dependency-injection-software-design-pattern"></a>Uso del modello di progettazione software per l'inserimento di dipendenze

Ora che è stata eseguita la migrazione del codice di accesso ai dati a una classe di repository separata, è necessario modificare il controller di contatto per usare questa classe. Si userà un modello di progettazione software denominato inserimento delle dipendenze per usare la classe di repository nel controller.

Il controller di contatto modificato è contenuto nel listato 3.

**Listato 3-Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Si noti che il controller di contatto nel listato 3 ha due costruttori. Il primo costruttore passa un'istanza concreta dell'interfaccia IContactManagerRepository al secondo costruttore. La classe Contact controller usa l' *inserimento delle dipendenze del costruttore*.

L'unica posizione in cui viene usata la classe EntityContactManagerRepository è nel primo costruttore. Il resto della classe usa l'interfaccia IContactManagerRepository anziché la classe EntityContactManagerRepository concreta.

In questo modo è più semplice cambiare le implementazioni della classe IContactManagerRepository in futuro. Se si vuole usare la classe DataServicesContactRepository anziché la classe EntityContactManagerRepository, è sufficiente modificare il primo costruttore.

L'inserimento di dipendenze del costruttore rende anche la classe del controller di contatto molto testabile. Negli unit test è possibile creare un'istanza del controller di contatto passando un'implementazione fittizia della classe IContactManagerRepository. Questa funzionalità di inserimento delle dipendenze sarà molto importante per Microsoft nell'iterazione successiva quando si compilano unit test per l'applicazione Contact Manager.

> [!NOTE] 
> 
> Se si vuole separare completamente la classe Contact Controller da una particolare implementazione dell'interfaccia IContactManagerRepository, è possibile sfruttare un Framework che supporta l'inserimento di dipendenze, ad esempio StructureMap o Microsoft Entity Framework (MEF). Sfruttando un Framework di inserimento delle dipendenze, non è mai necessario fare riferimento a una classe concreta nel codice.

## <a name="creating-a-service-layer"></a>Creazione di un livello di servizio

Probabilmente si è notato che la logica di convalida è ancora confusa con la logica del controller nella classe controller modificata nel listato 3. Per lo stesso motivo per cui è consigliabile isolare la logica di accesso ai dati, è consigliabile isolare la logica di convalida.

Per risolvere il problema, è possibile creare un [livello di servizio](http://martinfowler.com/eaaCatalog/serviceLayer.html)separato. Il livello di servizio è un livello separato che è possibile inserire tra le classi del controller e del repository. Il livello di servizio contiene la logica di business inclusa tutta la logica di convalida.

Il ContactManagerService è contenuto nel listato 4. Contiene la logica di convalida della classe Contact Controller.

**Listato 4-Models\ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Si noti che il costruttore per ContactManagerService richiede ValidationDictionary. Il livello di servizio comunica con il livello controller tramite questa ValidationDictionary. Quando si discute il modello di elemento Decorator, viene illustrato in dettaglio i ValidationDictionary nella sezione seguente.

Si noti inoltre che ContactManagerService implementa l'interfaccia IContactManagerService. Si consiglia di eseguire sempre la programmazione per le interfacce anziché le classi concrete. Altre classi nell'applicazione Contact Manager non interagiscono direttamente con la classe ContactManagerService. Al contrario, con un'eccezione, il resto dell'applicazione Contact Manager viene programmato con l'interfaccia IContactManagerService.

L'interfaccia IContactManagerService è inclusa nel listato 5.

**Listato 5-Models\IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

La classe del controller dei contatti modificata è inclusa nel listato 6. Si noti che il controller contatto non interagisce più con il repository ContactManager. Il controller di contatto interagisce invece con il servizio ContactManager. Ogni livello è isolato per quanto possibile da altri livelli.

**Elenco 6-Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

L'applicazione non esegue più afoul del principio di responsabilità singola (SRP). Il controller di contatto nel listato 6 è stato rimosso da qualsiasi responsabilità, ad eccezione del controllo del flusso dell'esecuzione dell'applicazione. Tutta la logica di convalida è stata rimossa dal controller dei contatti ed è stata inserita nel livello di servizio. È stato eseguito il push di tutte le logiche del database nel livello di repository.

## <a name="using-the-decorator-pattern"></a>Uso del modello Decorator

Si vuole essere in grado di separare completamente il livello di servizio dal livello del controller. In linea di principio, dovrebbe essere possibile compilare il livello di servizio in un assembly separato dal livello controller senza dover aggiungere un riferimento all'applicazione MVC.

Tuttavia, il livello di servizio deve essere in grado di passare nuovamente i messaggi di errore di convalida al livello del controller. Come è possibile abilitare il livello di servizio per la comunicazione dei messaggi di errore di convalida senza associare il controller e il livello di servizio? Possiamo sfruttare i vantaggi di un modello di progettazione software denominato [modello Decorator](http://en.wikipedia.org/wiki/Decorator_pattern).

Un controller usa un ModelStateDictionary denominato ModelState per rappresentare gli errori di convalida. Pertanto, si potrebbe essere tentati di passare ModelState dal livello controller al livello di servizio. Tuttavia, l'uso di ModelState nel livello di servizio renderebbe il livello di servizio dipendente da una funzionalità di ASP.NET MVC Framework. Questa operazione potrebbe non essere valida perché, un giorno, potrebbe essere necessario usare il livello di servizio con un'applicazione WPF anziché un'applicazione MVC ASP.NET. In tal caso, non è necessario fare riferimento a ASP.NET MVC Framework per usare la classe ModelStateDictionary.

Il modello Decorator consente di eseguire il wrapping di una classe esistente in una nuova classe per implementare un'interfaccia. Il progetto Contact Manager include la classe ModelStateWrapper contenuta nel listato 7. La classe ModelStateWrapper implementa l'interfaccia nel listato 8.

**Listato 7-Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Listato 8-Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Se si esamina l'elenco 5, si noterà che il livello di servizio ContactManager usa esclusivamente l'interfaccia IValidationDictionary. Il servizio ContactManager non dipende dalla classe ModelStateDictionary. Quando il controller di contatto crea il servizio ContactManager, il controller esegue il wrapping del ModelState come segue:

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Riepilogo

In questa iterazione non sono state aggiunte nuove funzionalità all'applicazione Contact Manager. L'obiettivo di questa iterazione era effettuare il refactoring dell'applicazione Contact Manager in modo che sia più facile da gestire e modificare.

In primo luogo, è stato implementato il modello di progettazione software del repository. È stata eseguita la migrazione di tutto il codice di accesso ai dati in una classe di repository ContactManager separata.

È stata anche isolata la logica di convalida dalla logica del controller. È stato creato un livello di servizio separato che contiene tutto il codice di convalida. Il livello controller interagisce con il livello di servizio e il livello di servizio interagisce con il livello di repository.

Quando è stato creato il livello di servizio, è stato sfruttato il modello Decorator per isolare ModelState dal livello di servizio. Nel livello di servizio è stato programmato l'interfaccia IValidationDictionary anziché ModelState.

Infine, è stato sfruttato un modello di progettazione software denominato modello di inserimento delle dipendenze. Questo modello consente di programmare in base a interfacce (astrazioni) anziché a classi concrete. L'implementazione del modello di progettazione per l'inserimento di dipendenze rende anche il codice più testabile. Nell'iterazione successiva si aggiungono unit test al progetto.

> [!div class="step-by-step"]
> [Precedente](iteration-3-add-form-validation-vb.md)
> [Successivo](iteration-5-create-unit-tests-vb.md)
