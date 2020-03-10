---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: usare Async e stored procedure con EF in un'app ASP.NET MVC"
description: In questa esercitazione viene illustrato come implementare il modello di programmazione asincrona e come utilizzare le stored procedure.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5612f2f25d06feb904a205505ed8f048d2263266
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583440"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a>Esercitazione: usare Async e stored procedure con EF in un'app ASP.NET MVC

Nelle esercitazioni precedenti si è appreso come leggere e aggiornare i dati utilizzando il modello di programmazione sincrona. In questa esercitazione viene illustrato come implementare il modello di programmazione asincrona. Il codice asincrono può migliorare le prestazioni di un'applicazione, in quanto consente di utilizzare al meglio le risorse del server.

In questa esercitazione viene inoltre illustrato come utilizzare le stored procedure per operazioni di inserimento, aggiornamento ed eliminazione in un'entità.

Infine, si ridistribuisce l'applicazione in Azure, insieme a tutte le modifiche apportate al database da quando è stata distribuita la prima volta.

Le figure seguenti illustrano alcune delle pagine che verranno usate.

![Pagina reparti](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Crea reparto](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Informazioni sul codice asincrono
> * Creare un controller di reparto
> * Utilizzare stored procedure
> * Distribuire in Azure

## <a name="prerequisites"></a>Prerequisiti

* [Aggiornamento di dati correlati](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a>Perché usare il codice asincrono

Per un server Web è disponibile un numero limitato di thread e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso. In queste circostanze il server non può elaborare nuove richieste finché i thread non saranno liberi. Con il codice sincrono, può succedere che molti thread siano vincolati nonostante in quel momento non stiano eseguendo alcuna operazione. Rimangono tuttavia in attesa che l'operazione I/O sia completata. Con il codice asincrono, se un processo è in attesa del completamento dell'operazione I/O, il thread viene liberato e il server lo può usare per l'elaborazione di altre richieste. Di conseguenza, il codice asincrono consente di usare le risorse del server in modo più efficiente e il server è abilitato per gestire più traffico senza ritardi.

Nelle versioni precedenti di .NET, la scrittura e il test del codice asincrono erano complessi, soggette a errori e difficili da debug. In .NET 4,5, la scrittura, il test e il debug del codice asincrono è molto più semplice che in genere è necessario scrivere codice asincrono a meno che non si abbia un motivo per non farlo. Il codice asincrono introduce una piccola quantità di overhead, ma per le situazioni di traffico ridotto il raggiungimento delle prestazioni è trascurabile, mentre per le situazioni di traffico elevato, il miglioramento delle prestazioni potenziale è sostanziale.

Per altre informazioni sulla programmazione asincrona, vedere [usare il supporto asincrono di .NET 4.5 per evitare di bloccare le chiamate](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-department-controller"></a>Crea controller reparto

Creare un controller di reparto nello stesso modo in cui sono stati usati i controller precedenti, ma questa volta selezionare la casella di controllo **use Async controller actions** .

Le evidenziazioni seguenti mostrano cosa è stato aggiunto al codice sincrono per il metodo `Index` per renderlo asincrono:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Sono state applicate quattro modifiche per consentire l'esecuzione asincrona della query Entity Framework database:

- Il metodo è contrassegnato con la parola chiave `async`, che indica al compilatore di generare callback per parti del corpo del metodo e di creare automaticamente l'oggetto `Task<ActionResult>` restituito.
- Il tipo restituito è stato modificato da `ActionResult` a `Task<ActionResult>`. Il tipo di `Task<T>` rappresenta il lavoro in corso con un risultato di tipo `T`.
- La parola chiave `await` è stata applicata alla chiamata al servizio Web. Quando il compilatore rileva questa parola chiave, dietro le quinte divide il metodo in due parti. La prima parte termina con l'operazione avviata in modo asincrono. La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.
- È stata chiamata la versione asincrona del metodo di estensione `ToList`.

Perché l'istruzione `departments.ToList` è stata modificata ma non l'istruzione `departments = db.Departments`? Il motivo è che solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono. L'istruzione `departments = db.Departments` imposta una query ma la query non viene eseguita fino a quando non viene chiamato il metodo `ToList`. Pertanto, solo il metodo `ToList` viene eseguito in modo asincrono.

Nel metodo `Details` e nei metodi `HttpGet` `Edit` e `Delete`, il metodo `Find` è quello che fa sì che venga inviata una query al database, in modo che sia il metodo che viene eseguito in modo asincrono:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

Nei metodi `Create`, `HttpPost Edit`e `DeleteConfirmed`, è la chiamata al metodo `SaveChanges` che determina l'esecuzione di un comando, non istruzioni come `db.Departments.Add(department)` che causano la modifica delle entità in memoria.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Aprire *Views\Department\Index.cshtml*e sostituire il codice del modello con il codice seguente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Questo codice modifica il titolo dall'indice ai reparti, sposta il nome amministratore a destra e fornisce il nome completo dell'amministratore.

Nelle visualizzazioni create, DELETE, Details e Edit modificare la didascalia per il campo `InstructorID` in "Administrator" nello stesso modo in cui il campo nome reparto è stato modificato in "Department" nelle visualizzazioni Course.

Nelle visualizzazioni crea e modifica usare il codice seguente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Nelle visualizzazioni Delete e Details usare il codice seguente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Eseguire l'applicazione e fare clic sulla scheda **reparti** .

Tutto funziona come negli altri controller, ma in questo controller tutte le query SQL vengono eseguite in modo asincrono.

Alcuni aspetti da tenere presenti quando si usa la programmazione asincrona con la Entity Framework:

- Il codice asincrono non è thread-safe. In altre parole, in altre parole, non provare a eseguire più operazioni in parallelo usando la stessa istanza del contesto.
- Se si vogliono sfruttare i vantaggi del codice asincrono in termini di prestazioni, verificare che i pacchetti della libreria impiegati, ad esempio per il paging, usino la modalità asincrona per chiamare i metodi di Entity Framework che generano query da inviare al database.

## <a name="use-stored-procedures"></a>Utilizzare stored procedure

Alcuni sviluppatori e DBA preferiscono utilizzare stored procedure per l'accesso al database. Nelle versioni precedenti di Entity Framework è possibile recuperare i dati utilizzando una stored procedure eseguendo [una query SQL non elaborata](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ma non è possibile indicare a EF di utilizzare stored procedure per le operazioni di aggiornamento. In EF 6 è facile configurare Code First per l'utilizzo di stored procedure.

1. In *DAL\SchoolContext.cs*aggiungere il codice evidenziato al metodo `OnModelCreating`.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Questo codice indica Entity Framework di utilizzare stored procedure per operazioni di inserimento, aggiornamento ed eliminazione sull'entità `Department`.
2. Nella console di gestione pacchetti immettere il comando seguente:

    `add-migration DepartmentSP`

    Aprire *migrazioni\\&lt;timestamp&gt;\_DepartmentSP.cs* per visualizzare il codice nel metodo `Up` che crea stored procedure di inserimento, aggiornamento ed eliminazione:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. Nella console di gestione pacchetti immettere il comando seguente:

     `update-database`
4. Eseguire l'applicazione in modalità di debug, fare clic sulla scheda **reparti** , quindi fare clic su **Crea nuovo**.
5. Immettere i dati per un nuovo reparto, quindi fare clic su **Crea**.

6. In Visual Studio esaminare i log nella finestra **output** per vedere che è stata usata una stored procedure per inserire la nuova riga del reparto.

     ![Reparto inserimento SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Code First crea nomi di stored procedure predefiniti. Se si utilizza un database esistente, potrebbe essere necessario personalizzare i nomi dei stored procedure per poter utilizzare le stored procedure già definite nel database. Per informazioni su come eseguire questa operazione, vedere [Entity Framework Code First stored procedure INSERT/UPDATE/DELETE](https://msdn.microsoft.com/data/dn468673).

Se si desidera personalizzare le stored procedure generate, è possibile modificare il codice con impalcature per le migrazioni `Up` metodo che crea l'stored procedure. In questo modo le modifiche vengono riflesse ogni volta che viene eseguita la migrazione e verranno applicate al database di produzione quando le migrazioni vengono eseguite automaticamente in produzione dopo la distribuzione.

Se si desidera modificare un stored procedure esistente creato in una migrazione precedente, è possibile utilizzare il comando Add-Migration per generare una migrazione vuota, quindi scrivere manualmente il codice che chiama il metodo [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) .

## <a name="deploy-to-azure"></a>Distribuire in Azure

Questa sezione richiede che sia stata completata la sezione facoltativa **Deploying the app to Azure** nell'esercitazione sulle [migrazioni e sulla distribuzione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) di questa serie. Se sono stati rilevati errori di migrazione risolti eliminando il database nel progetto locale, ignorare questa sezione.

1. In Visual Studio fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica** dal menu di scelta rapida.
2. Fare clic su **Pubblica**.

    Visual Studio distribuisce l'applicazione in Azure e l'applicazione viene aperta nel browser predefinito, in esecuzione in Azure.
3. Testare l'applicazione per verificarne il funzionamento.

    La prima volta che si esegue una pagina che accede al database, il Entity Framework esegue tutte le migrazioni `Up` metodi necessari per portare il database aggiornato con il modello di dati corrente. È ora possibile usare tutte le pagine Web aggiunte dall'ultima volta in cui è stata eseguita la distribuzione, incluse le pagine del reparto aggiunte in questa esercitazione.

## <a name="get-the-code"></a>Ottenere il codice

[Scaricare il progetto completato](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Risorse aggiuntive

I collegamenti ad altre risorse Entity Framework sono disponibili nelle [risorse consigliate per l'accesso ai dati ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Informazioni sul codice asincrono
> * Creazione di un controller di reparto
> * Stored procedure utilizzate
> * Distribuito in Azure

Passare all'articolo successivo per informazioni su come gestire i conflitti quando più utenti aggiornano la stessa entità nello stesso momento.
> [!div class="nextstepaction"]
> [Gestione della concorrenza](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
