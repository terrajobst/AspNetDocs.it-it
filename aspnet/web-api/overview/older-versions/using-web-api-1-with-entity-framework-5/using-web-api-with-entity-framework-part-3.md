---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: creazione di un controller di amministrazione | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600035"
---
# <a name="part-3-creating-an-admin-controller"></a>Parte 3: creazione di un controller di amministrazione

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Aggiungere un controller di amministrazione

In questa sezione si aggiungerà un controller API Web che supporta operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) sui prodotti. Il controller utilizzerà Entity Framework per comunicare con il livello di database. Solo gli amministratori saranno in grado di utilizzare questo controller. I clienti potranno accedere ai prodotti tramite un altro controller.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Controllers. Selezionare **Aggiungi** e quindi **controller**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

Nella finestra di dialogo **Aggiungi controller** assegnare un nome al controller `AdminController`. In **modello**selezionare &quot;controller API con azioni di lettura/scrittura, usando Entity Framework&quot;. In **classe modello**selezionare "Product (ProductStore. Models)". In **contesto dati**selezionare "&lt;nuovo contesto dati&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Se l'elenco a discesa **classe modello** non mostra alcuna classe del modello, assicurarsi di aver compilato il progetto. Entity Framework usa la reflection, quindi richiede l'assembly compilato.

Se si seleziona "&lt;nuovo contesto dati&gt;", viene aperta la finestra di dialogo **nuovo contesto dati** . Denominare il contesto dati `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Fare clic su **OK** per chiudere la finestra di dialogo **nuovo contesto dati** . Nella finestra di dialogo **Aggiungi controller** fare clic su **Aggiungi**.

Ecco cosa è stato aggiunto al progetto:

- Classe denominata `OrdersContext` che deriva da **DbContext**. Questa classe fornisce l'associazione tra i modelli POCO e il database.
- Un controller API Web denominato `AdminController`. Questo controller supporta le operazioni CRUD sulle istanze `Product`. Usa la classe `OrdersContext` per comunicare con Entity Framework.
- Nuova stringa di connessione del database nel file Web. config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Aprire il file OrdersContext.cs. Si noti che il costruttore specifica il nome della stringa di connessione del database. Questo nome fa riferimento alla stringa di connessione aggiunta al file Web. config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Aggiungere le proprietà seguenti alla classe `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Un **DbSet** rappresenta un set di entità su cui è possibile eseguire una query. Di seguito è riportato l'elenco completo per la classe `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

La classe `AdminController` definisce cinque metodi che implementano la funzionalità CRUD di base. Ogni metodo corrisponde a un URI che il client può richiamare:

| Controller (metodo) | Descrizione | URI | Metodo HTTP |
| --- | --- | --- | --- |
| GetProducts | Ottiene tutti i prodotti. | API/prodotti | GET |
| GetProduct | Trova un prodotto in base all'ID. | API/prodotti/*ID* | GET |
| PutProduct | Aggiorna un prodotto. | API/prodotti/*ID* | PUT |
| Postprodotto | Crea un nuovo prodotto. | API/prodotti | Inserisci |
| DeleteProduct | Elimina un prodotto. | API/prodotti/*ID* | DELETE |

Ogni metodo chiama `OrdersContext` per eseguire una query sul database. I metodi che modificano la chiamata della raccolta (PUT, POST e DELETE) `db.SaveChanges` per rendere permanente le modifiche al database. I controller vengono creati per ogni richiesta HTTP e quindi eliminati, quindi è necessario salvare le modifiche prima che un metodo venga restituito.

## <a name="add-a-database-initializer"></a>Aggiungere un inizializzatore di database

Entity Framework dispone di una funzionalità interessante che consente di popolare il database all'avvio e di ricreare automaticamente il database ogni volta che i modelli cambiano. Questa funzionalità è utile durante lo sviluppo, perché sono sempre presenti dati di test, anche se si modificano i modelli.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Models e creare una nuova classe denominata `OrdersContextInitializer`. Incollare l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Ereditando dalla classe **DropCreateDatabaseIfModelChanges** , viene indicato Entity Framework eliminare il database ogni volta che si modificano le classi del modello. Quando Entity Framework crea (o ricrea) il database, chiama il metodo **Seed** per popolare le tabelle. Viene usato il metodo **Seed** per aggiungere alcuni prodotti di esempio e un ordine di esempio.

Questa funzionalità è ideale per i test, ma non usa la classe **DropCreateDatabaseIfModelChanges** nell'ambiente di produzione, perché è possibile perdere i dati se un utente modifica una classe di modello.

Aprire quindi Global. asax e aggiungere il codice seguente al metodo di **avvio dell'applicazione\_** :

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Inviare una richiesta al controller

A questo punto, non è stato scritto alcun codice client, ma è possibile richiamare l'API Web usando un Web browser o uno strumento di debug HTTP come [Fiddler](http://www.fiddler2.com/fiddler2/). In Visual Studio premere F5 per avviare il debug. Il Web browser si aprirà `http://localhost:*portnum*/`, dove *portNum* è un numero di porta.

Inviare una richiesta HTTP a "`http://localhost:*portnum*/api/admin`. Il completamento della prima richiesta può risultare lento, perché Entity Framework necessario creare ed eseguire il seeding del database. La risposta dovrebbe essere simile alla seguente:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-2.md)
> [Successivo](using-web-api-with-entity-framework-part-4.md)
