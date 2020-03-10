---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: '#6 iterazione: usare lo sviluppo basato suC#test () | Microsoft Docs'
author: microsoft
description: In questa sesta iterazione vengono aggiunte nuove funzionalità all'applicazione scrivendo unit test prima e scrivendo il codice in base agli unit test. In questa iterazione,...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: aee0ff9d8d7f17e8a00dab12467bd3a3457fbe18
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601801"
---
# <a name="iteration-6--use-test-driven-development-c"></a>#6 iterazione: usare lo sviluppo basato suC#test ()

[Microsoft](https://github.com/microsoft)

[Scarica codice](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> In questa sesta iterazione vengono aggiunte nuove funzionalità all'applicazione scrivendo unit test prima e scrivendo il codice in base agli unit test. In questa iterazione vengono aggiunti gruppi di contatti.

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

Nell'iterazione precedente dell'applicazione Contact Manager sono stati creati unit test per fornire una rete di sicurezza per il codice. La motivazione per la creazione degli unit test consisteva nel rendere il codice più resiliente alle modifiche. Con gli unit test, è possibile apportare modifiche al codice e sapere immediatamente se le funzionalità esistenti sono state interrotte.

In questa iterazione si usano unit test per uno scopo completamente diverso. In questa iterazione si usano unit test come parte di una filosofia di progettazione dell'applicazione denominata *sviluppo basato su test*. Quando si pratica lo sviluppo basato su test, si scrivono prima i test e quindi si scrive il codice in base ai test.

Più precisamente, quando si pratica lo sviluppo basato su test, è necessario completare tre passaggi durante la creazione del codice (rosso/verde/refactor):

1. Scrivi un unit test che ha esito negativo (rosso)
2. Scrivi codice che passa il unit test (verde)
3. Effettuare il refactoring del codice (refactoring)

Prima di tutto, è necessario scrivere il unit test. Il unit test deve esprimere l'intenzione del comportamento del codice. Quando si crea il unit test per la prima volta, il unit test dovrebbe avere esito negativo. Il test avrà esito negativo perché non è stato ancora scritto alcun codice dell'applicazione che soddisfi il test.

Si scriverà quindi un codice sufficiente per consentire il passaggio del unit test. L'obiettivo è scrivere il codice in pigri, sloppiest e il modo più rapido possibile. È consigliabile evitare di sprecare tempo pensando all'architettura dell'applicazione. È invece opportuno concentrarsi sulla scrittura della quantità minima di codice necessaria per soddisfare l'intenzione espressa dal unit test.

Infine, dopo aver scritto un codice sufficiente, è possibile risalire e prendere in considerazione l'architettura complessiva dell'applicazione. In questo passaggio viene riscritto (refactoring) il codice sfruttando i modelli di progettazione software, ad esempio il modello di repository, in modo che il codice sia più gestibile. In questo passaggio è possibile riscrivere senza timore il codice perché il codice è coperto da unit test.

Sono molti i vantaggi derivanti dalla pratica dello sviluppo basato su test. Per prima cosa, lo sviluppo basato su test impone di concentrarsi sul codice effettivamente necessario per la scrittura. Poiché si è costantemente concentrati sulla scrittura di codice sufficiente per superare un determinato test, è possibile evitare di aggirare le erbacce e scrivere grandi quantità di codice che non verrà mai usato.

In secondo luogo, una metodologia di progettazione "test First" forza la scrittura di codice dal punto di vista del modo in cui verrà usato il codice. In altre parole, quando si pratica lo sviluppo basato su test, si scrivono costantemente i test dal punto di vista dell'utente. Lo sviluppo basato su test può pertanto produrre API più pulite e comprensibili.

Infine, lo sviluppo basato su test forza la scrittura di unit test come parte del normale processo di scrittura di un'applicazione. Come si avvicina alla scadenza di un progetto, il test è in genere il primo elemento che esce dalla finestra. Quando si pratica lo sviluppo basato su test, è più probabile che si abbia la possibilità di scrivere unit test perché lo sviluppo basato su test rende unit test centrali al processo di compilazione di un'applicazione.

> [!NOTE] 
> 
> Per altre informazioni sullo sviluppo basato su test, è consigliabile leggere il libro di Michael Feathers **funzionante con il codice legacy**.

In questa iterazione viene aggiunta una nuova funzionalità all'applicazione Contact Manager. Viene aggiunto il supporto per i gruppi di contatti. È possibile usare i gruppi di contatti per organizzare i contatti in categorie quali i gruppi di aziende e amici.

Questa nuova funzionalità verrà aggiunta all'applicazione seguendo un processo di sviluppo basato su test. Gli unit test verranno scritti per primi e tutti i codici verranno scritti in base a questi test.

## <a name="what-gets-tested"></a>Cosa viene testato

Come illustrato nell'iterazione precedente, in genere non si scrivono unit test per la logica di accesso ai dati o la logica di visualizzazione. Non è possibile scrivere unit test per la logica di accesso ai dati perché l'accesso a un database è un'operazione relativamente lenta. Non è possibile scrivere unit test per la logica di visualizzazione perché l'accesso a una vista richiede la rotazione di un server Web che è un'operazione relativamente lenta. Non è necessario scrivere un unit test a meno che il test non possa essere eseguito più volte molto rapidamente

Poiché lo sviluppo basato su test è gestito da unit test, si concentrano inizialmente sulla scrittura della logica di business e del controller. Si eviterà di toccare il database o le visualizzazioni. Microsoft non modificherà il database o creerà le visualizzazioni fino alla fine di questa esercitazione. Si inizierà con gli elementi che possono essere testati.

## <a name="creating-user-stories"></a>Creazione di storie utente

Quando si pratica lo sviluppo basato su test, si inizia sempre scrivendo un test. Viene immediatamente sollevata la domanda: come si decide quale test scrivere prima? Per rispondere a questa domanda, è necessario scrivere un set di [**storie utente**](http://en.wikipedia.org/wiki/User_stories).

Una storia utente è una descrizione molto breve (in genere una frase) di un requisito software. Deve trattarsi di una descrizione non tecnica di un requisito scritto dal punto di vista dell'utente.

Di seguito è riportato il set di storie utente che descrivono le funzionalità richieste dalla nuova funzionalità del gruppo di contatti:

1. L'utente può visualizzare un elenco di gruppi di contatti.
2. L'utente può creare un nuovo gruppo di contatti.
3. L'utente può eliminare un gruppo di contatti esistente.
4. Quando si crea un nuovo contatto, l'utente può selezionare un gruppo di contatti.
5. Quando si modifica un contatto esistente, l'utente può selezionare un gruppo di contatti.
6. Nella vista index verrà visualizzato un elenco di gruppi di contatti.
7. Quando un utente fa clic su un gruppo di contatti, viene visualizzato un elenco di contatti corrispondenti.

Si noti che questo elenco di storie utente è completamente comprensibile per un cliente. Non viene menzionato alcun dettaglio di implementazione tecnica.

Durante il processo di compilazione dell'applicazione, il set di storie utente può diventare più raffinato. È possibile suddividere una storia utente in più storie (requisiti). Ad esempio, è possibile decidere che la creazione di un nuovo gruppo di contatti debba includere la convalida. L'invio di un gruppo di contatti senza nome deve restituire un errore di convalida.

Dopo aver creato un elenco di storie utente, si è pronti per scrivere il primo unit test. Si inizierà creando un unit test per visualizzare l'elenco dei gruppi di contatti.

## <a name="listing-contact-groups"></a>Elenco di gruppi di contatti

La prima storia utente è che un utente dovrebbe essere in grado di visualizzare un elenco di gruppi di contatti. È necessario esprimere questa storia con un test.

Per creare un nuovo unit test, fare clic con il pulsante destro del mouse sulla cartella Controllers nel progetto ContactManager. tests, scegliere **Aggiungi, nuovo test**e selezionare il modello di **unit test** (vedere la figura 1). Assegnare al nuovo unit test il nome GroupControllerTest.cs e fare clic sul pulsante **OK** .

[![aggiunta della unit test GroupControllerTest](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**Figura 01**: aggiunta del unit test GroupControllerTest ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-6-use-test-driven-development-cs/_static/image2.png))

Il primo unit test è contenuto nel listato 1. Questo test verifica che il metodo index () del controller di gruppo restituisca un set di gruppi. Il test verifica che una raccolta di gruppi venga restituita nei dati della visualizzazione.

**Listato 1-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

Quando si digita per la prima volta il codice nel listato 1 in Visual Studio, si otterrà una grande quantità di righe rosse ondulate. Non sono state create le classi GroupController o Group.

A questo punto, non è possibile compilare l'applicazione in modo che sia possibile eseguire la prima unit test. Questo è valido. Questo numero viene conteggiato come test con esito negativo. Per questo motivo, ora è possibile iniziare a scrivere il codice dell'applicazione. È necessario scrivere codice sufficiente per eseguire il test.

La classe controller di gruppo nel listato 2 contiene il minimo del codice necessario per passare il unit test. L'azione index () restituisce un elenco di gruppi codificato in modo statico (la classe Group è definita nel listato 3).

**Listato 2-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**Listato 3-Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

Dopo aver aggiunto le classi GroupController e Group al progetto, il primo unit test viene completato correttamente (vedere la figura 2). È stato eseguito il lavoro minimo necessario per superare il test. È il momento di festeggiare.

[![esito positivo.](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**Figura 02**: operazione riuscita. ([Fare clic per visualizzare l'immagine con dimensioni complete](iteration-6-use-test-driven-development-cs/_static/image4.png))

## <a name="creating-contact-groups"></a>Creazione di gruppi di contatti

A questo punto è possibile passare alla seconda storia utente. È necessario essere in grado di creare nuovi gruppi di contatti. È necessario esprimere questa intenzione con un test.

Il test nel listato 4 Verifica che chiamando il metodo Create () con un nuovo gruppo venga aggiunto il gruppo all'elenco di gruppi restituiti dal metodo index (). In altre parole, se si crea un nuovo gruppo, sarà possibile riportare il nuovo gruppo dall'elenco dei gruppi restituiti dal metodo index ().

**Listato 4-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

Il test nel listato 4 chiama il metodo del controller di gruppo create () con un nuovo gruppo di contatti. Successivamente, il test verifica che la chiamata del metodo index controller () restituisca il nuovo gruppo nei dati della visualizzazione.

Il controller di gruppo modificato nel listato 5 contiene le modifiche minime necessarie per superare il nuovo test.

**Listato 5-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Il controller di gruppo nel listato 5 include una nuova azione crea (). Questa azione aggiunge un gruppo a una raccolta di gruppi. Si noti che l'azione index () è stata modificata per restituire il contenuto della raccolta di gruppi.

Ancora una volta, è stata eseguita la quantità minima di lavoro necessaria per passare il unit test. Dopo aver apportato queste modifiche al controller gruppo, tutti gli unit test vengono superati.

## <a name="adding-validation"></a>Aggiunta della convalida

Questo requisito non è stato dichiarato in modo esplicito nella storia utente. Tuttavia, è ragionevole richiedere che un gruppo disponga di un nome. In caso contrario, non sarà molto utile organizzare i contatti in gruppi.

L'elenco 6 contiene un nuovo test che esprime questa intenzione. Questo test verifica che il tentativo di creare un gruppo senza specificare un nome genera un messaggio di errore di convalida nello stato del modello.

**Elenco 6-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

Per soddisfare questo test, è necessario aggiungere una proprietà Name alla classe Group (vedere il listato 7). Inoltre, è necessario aggiungere una piccola parte della logica di convalida all'azione crea () del controller del gruppo (vedere l'elenco 8).

**Listato 7-Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**Listato 8-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

Si noti che l'azione del controller di gruppo create () contiene ora sia la convalida che la logica del database. Attualmente, il database utilizzato dal controller di gruppo è costituito solo da una raccolta in memoria.

## <a name="time-to-refactor"></a>Tempo per effettuare il refactoring

Il terzo passaggio in rosso/verde/refactoring è la parte refactor. A questo punto, è necessario eseguire il refactoring del codice e prendere in considerazione il modo in cui è possibile effettuare il refactoring dell'applicazione per migliorare la progettazione. La fase di refactoring è la fase in cui si ritiene che sia il modo migliore per implementare i principi e i modelli di progettazione software.

È possibile modificare il codice in qualsiasi modo in cui si sceglie di migliorare la progettazione del codice. È presente una rete di sicurezza di unit test che impedisce di suddividere le funzionalità esistenti.

Al momento, il nostro controller di gruppo è un pasticcio dal punto di vista della buona progettazione del software. Il controller di gruppo contiene un disordine confuso della convalida e del codice di accesso ai dati. Per evitare di violare il principio di singola responsabilità, è necessario separare questi aspetti in classi diverse.

La classe del controller di gruppo sottoposto a refactoring è inclusa nel listato 9. Il controller è stato modificato per usare il livello del servizio ContactManager. Si tratta dello stesso livello di servizio usato con il controller di contatto.

L'elenco 10 contiene i nuovi metodi aggiunti al livello di servizio ContactManager per supportare la convalida, l'elenco e la creazione di gruppi. L'interfaccia IContactManagerService è stata aggiornata per includere i nuovi metodi.

L'elenco 11 contiene una nuova classe FakeContactManagerRepository che implementa l'interfaccia IContactManagerRepository. A differenza della classe EntityContactManagerRepository che implementa anche l'interfaccia IContactManagerRepository, la nuova classe FakeContactManagerRepository non comunica con il database. La classe FakeContactManagerRepository usa una raccolta in memoria come proxy per il database. Questa classe verrà usata negli unit test come un falso livello di repository.

**Listato 9-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**Listato 10-Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**Listato 11-Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

Per modificare l'interfaccia IContactManagerRepository, è necessario utilizzare per implementare i metodi CreateGroup () e ListGroups () nella classe EntityContactManagerRepository. Il metodo pigri e il modo più rapido per eseguire questa operazione consiste nell'aggiungere metodi stub simili ai seguenti:   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]

Infine, queste modifiche alla progettazione dell'applicazione richiedono di apportare alcune modifiche agli unit test. È ora necessario usare FakeContactManagerRepository quando si eseguono gli unit test. La classe GroupControllerTest aggiornata è inclusa nel listato 12.

**Listato 12-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

Dopo aver apportato tutte le modifiche, anche in questo caso tutti gli unit test vengono superati. È stato completato l'intero ciclo di rosso/verde/refactor. Sono state implementate le prime due storie utente. Sono ora disponibili unit test di supporto per i requisiti espressi nelle storie utente. L'implementazione del resto delle storie utente comporta la ripetizione dello stesso ciclo di rosso/verde/refactoring.

## <a name="modifying-our-database"></a>Modifica del database

Purtroppo, anche se sono stati soddisfatti tutti i requisiti espressi dagli unit test, il lavoro non viene eseguito. È ancora necessario modificare il database.

È necessario creare una nuova tabella di database di gruppo. Attenersi ai passaggi riportati di seguito.

1. Nella finestra Esplora server fare clic con il pulsante destro del mouse sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**.
2. Immettere le due colonne descritte di seguito nella Progettazione tabelle.
3. Contrassegnare la colonna ID come una chiave primaria e una colonna Identity.
4. Salvare la nuova tabella con i gruppi di nomi facendo clic sull'icona del floppy.

<a id="0.11_table01"></a>

| **Nome colonna** | **Tipo di dati** | **Consenti valori NULL** |
| --- | --- | --- |
| Id | int | False |
| NOME | nvarchar(50) | False |

Successivamente, è necessario eliminare tutti i dati dalla tabella Contacts. in caso contrario, non sarà possibile creare una relazione tra le tabelle Contacts e groups. Attenersi ai passaggi riportati di seguito.

1. Fare clic con il pulsante destro del mouse sulla tabella Contacts e selezionare l'opzione di menu **Mostra dati tabella**.
2. Elimina tutte le righe.

Successivamente, è necessario definire una relazione tra la tabella del database dei gruppi e la tabella del database dei contatti esistente. Attenersi ai passaggi riportati di seguito.

1. Fare doppio clic sulla tabella Contacts nella finestra di Esplora server per aprire il Progettazione tabelle.
2. Aggiungere una nuova colonna integer alla tabella Contacts denominata GroupId.
3. Fare clic sul pulsante relazione per aprire la finestra di dialogo relazioni di chiave esterna (vedere la figura 3).
4. Fare clic sul pulsante Aggiungi.
5. Fare clic sul pulsante con i puntini di sospensione visualizzato accanto al pulsante specifica tabella e colonne.
6. Nella finestra di dialogo tabelle e colonne selezionare gruppi come tabella di chiave primaria e ID come colonna chiave primaria. Selezionare Contacts come tabella di chiave esterna e GroupId come colonna chiave esterna (vedere la figura 4). Fare clic sul pulsante OK.
7. In **specifica Inserisci e aggiorna**selezionare il valore **Cascade** per **regola di eliminazione**.
8. Fare clic sul pulsante Chiudi per chiudere la finestra di dialogo relazioni di chiave esterna.
9. Fare clic sul pulsante Salva per salvare le modifiche apportate alla tabella Contacts.

[![la creazione di una relazione tra tabelle di database](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**Figura 03**: creazione di una relazione tra tabelle[di database (fare clic per visualizzare l'immagine con dimensioni complete](iteration-6-use-test-driven-development-cs/_static/image6.png))

[![specifica delle relazioni tra tabelle](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**Figura 04**: specifica delle relazioni tra tabelle ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-6-use-test-driven-development-cs/_static/image8.png))

### <a name="updating-our-data-model"></a>Aggiornamento del modello di dati

Successivamente, è necessario aggiornare il modello di dati per rappresentare la nuova tabella di database. Attenersi ai passaggi riportati di seguito.

1. Fare doppio clic sul file ContactManagerModel. edmx nella cartella Models per aprire il Entity Designer.
2. Fare clic con il pulsante destro del mouse sull'area di progettazione e selezionare l'opzione **di menu Aggiorna modello da database**.
3. Nell'aggiornamento guidato selezionare la tabella gruppi e fare clic sul pulsante fine (vedere la figura 5).
4. Fare clic con il pulsante destro del mouse sull'entità Groups e selezionare l'opzione di menu **Rename**. Modificare il nome dell'entità *Groups* in *Group* (Singular).
5. Fare clic con il pulsante destro del mouse sulla proprietà di navigazione gruppi visualizzata nella parte inferiore dell'entità Contact. Modificare il nome della proprietà di navigazione *gruppi* in *gruppo* (singolare).

[![l'aggiornamento di un modello di Entity Framework dal database](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**Figura 05**: aggiornamento di un modello di Entity Framework dal database ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-6-use-test-driven-development-cs/_static/image10.png))

Dopo aver completato questi passaggi, il modello di dati rappresenterà le tabelle contatti e gruppi. Il Entity Designer dovrebbe mostrare entrambe le entità (vedere la figura 6).

[![Entity Designer visualizzare il gruppo e il contatto](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**Figura 06**: Entity Designer visualizzare il gruppo e il contatto ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-6-use-test-driven-development-cs/_static/image12.png))

### <a name="creating-our-repository-classes"></a>Creazione delle classi di repository

Successivamente, è necessario implementare la classe del repository. Nel corso di questa iterazione sono stati aggiunti diversi nuovi metodi all'interfaccia IContactManagerRepository durante la scrittura del codice per soddisfare gli unit test. La versione finale dell'interfaccia IContactManagerRepository è contenuta nel listato 14.

**Listato 14-Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

In realtà non sono stati implementati i metodi correlati all'uso dei gruppi di contatti. Attualmente la classe EntityContactManagerRepository dispone di metodi stub per ognuno dei metodi del gruppo di contatti elencati nell'interfaccia IContactManagerRepository. Ad esempio, il metodo ListGroups () ha un aspetto simile al seguente:

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

I metodi stub ci hanno permesso di compilare l'applicazione e di passare gli unit test. Tuttavia, a questo punto è necessario implementare effettivamente questi metodi. La versione finale della classe EntityContactManagerRepository è contenuta nel listato 13.

**Listato 13-Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>Creazione di viste

Applicazione MVC ASP.NET quando si usa il motore di visualizzazione ASP.NET predefinito. Non è quindi possibile creare visualizzazioni in risposta a un particolare unit test. Tuttavia, poiché un'applicazione sarebbe inutile senza viste, è possibile completare questa iterazione senza creare e modificare le visualizzazioni contenute nell'applicazione Contact Manager.

È necessario creare le nuove viste seguenti per la gestione dei gruppi di contatti (vedere la figura 7):

- Views\Group\Index.aspx-Visualizza l'elenco dei gruppi di contatti
- Views\Group\Delete.aspx-Visualizza il modulo di conferma per l'eliminazione di un gruppo di contatti

[![la visualizzazione dell'indice del gruppo](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**Figura 07**: visualizzazione dell'indice del gruppo ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-6-use-test-driven-development-cs/_static/image14.png))

È necessario modificare le viste esistenti seguenti in modo da includere i gruppi di contatti:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

È possibile visualizzare le viste modificate esaminando l'applicazione di Visual Studio che accompagna questa esercitazione. Ad esempio, nella figura 8 viene illustrata la visualizzazione dell'indice dei contatti.

[![visualizzazione indice contatti](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**Figura 08**: visualizzazione dell'indice dei contatti ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-6-use-test-driven-development-cs/_static/image16.png))

## <a name="summary"></a>Riepilogo

In questa iterazione sono state aggiunte nuove funzionalità all'applicazione Contact Manager seguendo una metodologia di progettazione delle applicazioni per lo sviluppo basato su test. È stato creato un set di storie utente. È stato creato un set di unit test che corrisponde ai requisiti espressi dalle storie utente. Infine, è stato scritto il codice sufficiente per soddisfare i requisiti espressi dagli unit test.

Al termine della scrittura di codice sufficiente per soddisfare i requisiti espressi dagli unit test, il database e le visualizzazioni sono stati aggiornati. È stata aggiunta una nuova tabella di gruppi al database e il modello di dati Entity Framework è stato aggiornato. È stato inoltre creato e modificato un set di visualizzazioni.

Nell'iterazione successiva, ovvero l'iterazione finale, l'applicazione viene riscritta per sfruttare i vantaggi di AJAX. Sfruttando i vantaggi di AJAX, verranno migliorati la velocità di risposta e le prestazioni dell'applicazione Contact Manager.

> [!div class="step-by-step"]
> [Precedente](iteration-5-create-unit-tests-cs.md)
> [Successivo](iteration-7-add-ajax-functionality-cs.md)
