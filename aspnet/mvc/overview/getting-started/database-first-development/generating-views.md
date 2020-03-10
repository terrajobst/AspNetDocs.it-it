---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: "Esercitazione: generare visualizzazioni per Database First EF con l'app MVC ASP.NET"
description: Questa esercitazione è incentrata sull'uso dell'impalcatura ASP.NET per generare i controller e le visualizzazioni.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e71e13e22d8a72e1699cfc70d4d93af603edba5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616207"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Esercitazione: generare visualizzazioni per Database First EF con l'app MVC ASP.NET

Usando l'impalcatura MVC, Entity Framework e ASP.NET, è possibile creare un'applicazione Web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare automaticamente il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database.

Questa esercitazione è incentrata sull'uso dell'impalcatura ASP.NET per generare i controller e le visualizzazioni.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungi impalcatura
> * Aggiungere collegamenti a nuove visualizzazioni
> * Visualizzare le visualizzazioni degli studenti
> * Visualizzare le visualizzazioni di registrazione

## <a name="prerequisite"></a>Prerequisito

* [Creare l'applicazione Web e i modelli di dati](creating-the-web-application.md)

## <a name="add-scaffold"></a>Aggiungi impalcatura

Si è pronti per generare il codice che fornirà operazioni dati standard per le classi del modello. Per aggiungere il codice, è necessario aggiungere un elemento del patibolo. Sono disponibili molte opzioni per il tipo di impalcatura che è possibile aggiungere; in questa esercitazione il patibolo includerà un controller e le visualizzazioni che corrispondono ai modelli Student e di registrazione creati nella sezione precedente.

Per mantenere la coerenza nel progetto, il nuovo controller viene aggiunto alla cartella dei **controller** esistenti. Fare clic con il pulsante destro del mouse sulla cartella **controller** e scegliere **Aggiungi** > **nuovo elemento con impalcatura**.

Selezionare il **controller MVC 5 con le visualizzazioni, usando Entity Framework** opzione. Questa opzione consente di generare il controller e le visualizzazioni per l'aggiornamento, l'eliminazione, la creazione e la visualizzazione dei dati nel modello.

![Aggiungi controller MVC](generating-views/_static/image2.png)

Selezionare **Student (ContosoSite. Models)** per la classe Model e selezionare **ContosoUniversityDataEntities (ContosoSite. Models)** per la classe Context. Mantieni il nome del controller come **StudentsController**.

Fare clic su **Aggiungi**.

Se viene visualizzato un errore, è possibile che non sia stato compilato il progetto nella sezione precedente. In tal caso, provare a compilare il progetto, quindi aggiungere nuovamente l'elemento con impalcatura.

Al termine del processo di generazione del codice, verrà visualizzato un nuovo controller e visualizzazioni nei **controller** e nelle **visualizzazioni** del progetto > cartelle **students** .

Eseguire di nuovo la stessa procedura, ma aggiungere un patibolo per la classe di **registrazione** . Al termine, si disporrà di un file **EnrollmentsController.cs** e di una cartella in **visualizzazioni** denominate **registrazioni** con le viste create, DELETE, Details, Edit e index.

## <a name="add-links-to-new-views"></a>Aggiungere collegamenti a nuove visualizzazioni

Per semplificare l'esplorazione delle nuove visualizzazioni, è possibile aggiungere un paio di collegamenti ipertestuali alle viste degli indici per studenti e registrazioni. Aprire il file nelle **viste** > **Home** > *index. cshtml*, che corrisponde al Home page per il sito. Aggiungere il codice seguente sotto Jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Per il metodo ActionLink, il primo parametro è il testo da visualizzare nel collegamento. Il secondo parametro è l'azione e il terzo parametro è il nome del controller. Il primo collegamento, ad esempio, punta all'azione index in StudentsController. Il collegamento ipertestuale effettivo viene costruito in base a questi valori. Il primo collegamento porta gli utenti al file **index. cshtml** all'interno della cartella **views/students** .

## <a name="display-student-views"></a>Visualizzare le visualizzazioni degli studenti

Si verificherà che il codice aggiunto al progetto visualizzi correttamente un elenco degli studenti e consente agli utenti di modificare, creare o eliminare i record degli studenti nel database.

Fare clic con il pulsante destro del **mouse sul file** > **Home** > *index. cshtml* , quindi scegliere **Visualizza nel browser**. Nella home page dell'applicazione selezionare **elenco di studenti**.

![](generating-views/_static/image6.png)

Nella pagina **index** si noti l'elenco degli studenti e dei collegamenti per modificare questi dati. Selezionare il collegamento **Crea nuovo** e specificare alcuni valori per un nuovo studente. Fare clic su **Crea**. si noti che il nuovo studente viene aggiunto all'elenco.

Tornare alla pagina **index** , selezionare il collegamento **Edit (modifica** ) e modificare alcuni dei valori di uno studente. Fare clic su **Save (Salva**). si noti che il record Student è stato modificato.

Infine, selezionare il collegamento **Elimina** e confermare che si desidera eliminare il record facendo clic sul pulsante **Elimina** .

Senza scrivere codice, sono state aggiunte viste che eseguono operazioni comuni sui dati della tabella Student.

Si può notare che l'etichetta di testo per un campo è basata sulla proprietà del database, ad esempio **LastName**, che non è necessariamente ciò che si desidera visualizzare nella pagina Web. Ad esempio, è possibile preferire l'etichetta come **Cognome**. Questo problema di visualizzazione verrà risolto più avanti nell'esercitazione.

## <a name="display-enrollment-views"></a>Visualizzare le visualizzazioni di registrazione

Il database include una relazione uno-a-molti tra le tabelle Student e di registrazione e una relazione uno-a-molti tra il corso e le tabelle di registrazione. Le visualizzazioni per la registrazione gestiscono correttamente queste relazioni. Passare alla home page per il sito e selezionare il collegamento **elenco di registrazioni** , quindi il collegamento **Crea nuovo** .

La vista consente di visualizzare un modulo per la creazione di un nuovo record di registrazione. In particolare, si noti che il modulo contiene un elenco a discesa **CourseID** e un elenco a discesa **StudentID** . Entrambe le tabelle vengono popolate con i valori delle tabelle correlate.

Inoltre, la convalida dei valori forniti viene applicata automaticamente in base al tipo di dati del campo. Il **livello** richiede un numero, pertanto viene visualizzato un messaggio di errore se si tenta di fornire un valore incompatibile: *il livello di campo deve essere un numero.*

Si è verificato che le visualizzazioni generate automaticamente consentono agli utenti di utilizzare i dati nel database. Nell'esercitazione successiva di questa serie si aggiornerà il database e si apporteranno le modifiche corrispondenti nell'applicazione Web.

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Impalcatura aggiunta
> * Aggiunta di collegamenti a nuove visualizzazioni
> * Visualizzazioni degli studenti visualizzate
> * Visualizzazioni di registrazione visualizzate

Passare all'esercitazione successiva per apprendere come modificare il database.
> [!div class="nextstepaction"]
> [Modificare il database](changing-the-database.md)