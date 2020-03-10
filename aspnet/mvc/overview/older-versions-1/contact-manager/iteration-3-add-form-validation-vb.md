---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: '#3 iterazione-aggiungere la convalida del modulo (VB) | Microsoft Docs'
author: microsoft
description: Nella terza iterazione viene aggiunta la convalida dei form di base. Si impedisce agli utenti di inviare un modulo senza completare i campi dei moduli richiesti. Viene anche convalidato emai...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 73b307f53875abe84b592c75b1ff614ffd9d8b82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544450"
---
# <a name="iteration-3--add-form-validation-vb"></a>#3 iterazione-Aggiungi convalida Form (VB)

[Microsoft](https://github.com/microsoft)

[Scarica codice](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> Nella terza iterazione viene aggiunta la convalida dei form di base. Si impedisce agli utenti di inviare un modulo senza completare i campi dei moduli richiesti. Vengono convalidati anche gli indirizzi di posta elettronica e i numeri di telefono.

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

In questa seconda iterazione dell'applicazione Contact Manager, viene aggiunta la convalida dei form di base. Si impedisce agli utenti di inviare un contatto senza immettere valori per i campi modulo richiesti. Vengono convalidati anche i numeri di telefono e gli indirizzi di posta elettronica (vedere la figura 1).

[![finestra di dialogo nuovo progetto](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Figura 01**: un modulo con convalida ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-3-add-form-validation-vb/_static/image2.png))

In questa iterazione viene aggiunta la logica di convalida direttamente alle azioni del controller. In generale, questo non è il metodo consigliato per aggiungere la convalida a un'applicazione MVC ASP.NET. Un approccio migliore consiste nell'inserire la logica di convalida di un'applicazione in un [livello di servizio](http://martinfowler.com/eaaCatalog/serviceLayer.html)separato. Nell'iterazione successiva si effettua il refactoring dell'applicazione Contact Manager per rendere l'applicazione più gestibile.

In questa iterazione, per semplificare l'operazione, si scrive tutto il codice di convalida manualmente. Invece di scrivere il codice di convalida, possiamo usufruire di un Framework di convalida. Ad esempio, è possibile usare Microsoft Enterprise Library Validation Application Block (VAB) per implementare la logica di convalida per l'applicazione ASP.NET MVC. Per ulteriori informazioni sul blocco applicazione di convalida, vedere:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Aggiunta della convalida alla visualizzazione di creazione

Per iniziare, è necessario aggiungere la logica di convalida alla vista Create. Fortunatamente, poiché la creazione della vista è stata generata con Visual Studio, la vista Create contiene già tutta la logica dell'interfaccia utente necessaria per visualizzare i messaggi di convalida. La vista Create è inclusa nel listato 1.

**Listato 1-\Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

La classe Field-Validation-Error viene usata per definire lo stile dell'output sottoposto a rendering dall'helper HTML. ValidationMessage (). La classe Input-Validation-Error viene usata per applicare uno stile alla casella di testo (input) sottoposta a rendering dall'helper HTML. TextBox (). La classe Validation-Summary-Errors viene usata per definire lo stile dell'elenco non ordinato di cui è stato eseguito il rendering dall'helper HTML. ValidationSummary ().

> [!NOTE] 
> 
> È possibile modificare le classi del foglio di stile descritte in questa sezione per personalizzare l'aspetto dei messaggi di errore di convalida.

## <a name="adding-validation-logic-to-the-create-action"></a>Aggiunta della logica di convalida all'azione di creazione

A questo punto, la vista crea non visualizza mai i messaggi di errore di convalida perché non è stata scritta la logica per generare messaggi. Per visualizzare i messaggi di errore di convalida, è necessario aggiungere i messaggi di errore a ModelState.

> [!NOTE] 
> 
> Il metodo UpdateModel () aggiunge automaticamente i messaggi di errore a ModelState quando si verifica un errore durante l'assegnazione del valore di un campo del modulo a una proprietà. Se ad esempio si tenta di assegnare la stringa "Apple" a una proprietà di data di nascita che accetta valori DateTime, il metodo UpdateModel () aggiunge un errore a ModelState.

Il metodo Create () modificato nel listato 2 contiene una nuova sezione che convalida le proprietà della classe Contact prima che il nuovo contatto venga inserito nel database.

**Listato 2-Controllers\ContactController.vb (crea con convalida)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

La sezione Validate impone quattro regole di convalida distinte:

- La lunghezza della proprietà FirstName deve essere maggiore di zero e non può essere costituita solo da spazi.
- La lunghezza della proprietà LastName deve essere maggiore di zero e non può essere costituita solo da spazi.
- Se la proprietà Phone ha un valore (ha una lunghezza maggiore di 0), la proprietà Phone deve corrispondere a un'espressione regolare.
- Se la proprietà email ha un valore (ha una lunghezza maggiore di 0), la proprietà email deve corrispondere a un'espressione regolare.

Quando si verifica una violazione della regola di convalida, viene aggiunto un messaggio di errore a ModelState con la guida del Metodo AddModelError (). Quando si aggiunge un messaggio a ModelState, si specifica il nome di una proprietà e il testo di un messaggio di errore di convalida. Questo messaggio di errore viene visualizzato nella visualizzazione dai metodi helper HTML. ValidationSummary () e HTML. ValidationMessage ().

Dopo l'esecuzione delle regole di convalida, viene verificata la proprietà IsValid di ModelState. La proprietà IsValid restituisce false quando sono stati aggiunti messaggi di errore di convalida a ModelState. Se la convalida ha esito negativo, il modulo di creazione viene visualizzato nuovamente con i messaggi di errore.

> [!NOTE] 
> 
> Sono state ottenute le espressioni regolari per convalidare il numero di telefono e l'indirizzo di posta elettronica dal repository di espressioni regolari all' [ *http://regexlib.com* ](http://regexlib.com)

## <a name="adding-validation-logic-to-the-edit-action"></a>Aggiunta della logica di convalida all'azione di modifica

L'azione Edit () aggiorna un contatto. L'azione Edit () deve eseguire esattamente la stessa convalida dell'azione Create (). Anziché duplicare lo stesso codice di convalida, è necessario effettuare il refactoring del controller di contatto in modo che entrambe le azioni create () e Edit () chiamino lo stesso metodo di convalida.

La classe del controller dei contatti modificata è inclusa nell'elenco 3. Questa classe dispone di un nuovo metodo ValidateContact () che viene chiamato all'interno delle azioni create () e Edit ().

**Listato 3-Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Riepilogo

In questa iterazione è stata aggiunta la convalida dei form di base all'applicazione Contact Manager. La logica di convalida impedisce agli utenti di inviare un nuovo contatto o di modificare un contatto esistente senza specificare i valori per le proprietà FirstName e LastName. Inoltre, gli utenti devono specificare numeri di telefono e indirizzi di posta elettronica validi.

In questa iterazione è stata aggiunta la logica di convalida all'applicazione Contact Manager nel modo più semplice possibile. Tuttavia, la combinazione della logica di convalida nella logica del controller creerà problemi a lungo termine. L'applicazione sarà più difficile da gestire e modificare nel tempo.

Nell'iterazione successiva verrà effettuato il refactoring della logica di convalida e della logica di accesso al database dai controller. Si approfitteranno dei diversi principi di progettazione software per consentire a Microsoft di creare un'applicazione più a regime di controllo libero e più gestibile.

> [!div class="step-by-step"]
> [Precedente](iteration-2-make-the-application-look-nice-vb.md)
> [Successivo](iteration-4-make-the-application-loosely-coupled-vb.md)
