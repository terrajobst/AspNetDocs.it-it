---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usare Migrazioni Code First per il seeding del database | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557456"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a>Usare Migrazioni Code First per inizializzare il database

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si userà [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) in EF per eseguire il seeding del database con i dati di test.

Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**. Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](part-3/samples/sample1.cmd)]

Questo comando aggiunge una cartella denominata Migrations al progetto, oltre a un file di codice denominato Configuration.cs nella cartella Migrations.

![](part-3/_static/image1.png)

Aprire il file Configuration.cs. Aggiungere la seguente istruzione **using** .

[!code-csharp[Main](part-3/samples/sample2.cs)]

Aggiungere quindi il codice seguente al metodo **Configuration. Seed** :

[!code-csharp[Main](part-3/samples/sample3.cs)]

Nella finestra console di gestione pacchetti digitare i comandi seguenti:

[!code-console[Main](part-3/samples/sample4.cmd)]

Il primo comando genera il codice che crea il database e il secondo comando esegue tale codice. Il database viene creato localmente, utilizzando il database [locale](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Esplorare l'API (facoltativo)

Premere F5 per eseguire l'applicazione in modalità debug. Visual Studio avvia IIS Express ed esegue l'app Web. Visual Studio avvia quindi un browser e apre il home page dell'app.

Quando Visual Studio esegue un progetto Web, assegna un numero di porta. Nell'immagine seguente il numero di porta è 50524. Quando si esegue l'applicazione, viene visualizzato un numero di porta diverso.

![](part-3/_static/image3.png)

Il home page viene implementato con ASP.NET MVC. Nella parte superiore della pagina è presente un collegamento che indica "API". Questo collegamento consente di portare a una pagina della guida generata automaticamente per l'API Web. Per informazioni sul modo in cui viene generata questa pagina della guida e su come è possibile aggiungere la propria documentazione alla pagina, vedere [creazione di pagine della Guida per API Web ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md). È possibile fare clic sui collegamenti della pagina della Guida per visualizzare i dettagli relativi all'API, incluso il formato della richiesta e della risposta.

![](part-3/_static/image4.png)

L'API Abilita le operazioni CRUD sul database. Di seguito viene riepilogata l'API.

| Autori |  |
| --- | -- |
| GET api/authors | Ottenere tutti gli autori. |
| GET api/authors/{id} | Ottenere un autore in base all'ID. |
| POST /api/authors | Creare un nuovo autore. |
| PUT /api/authors/{id} | Aggiornare un autore esistente. |
| DELETE /api/authors/{id} | Eliminare un autore. |

| Libri |  |
| --- | -- |
| GET /api/books | Ottenere tutti i libri. |
| GET /api/books/{id} | Ottenere un libro in base all'ID. |
| POST /api/books | Creare un nuovo libro. |
| PUT /api/books/{id} | Aggiornare un libro esistente. |
| DELETE /api/books/{id} | Eliminare un libro. |

## <a name="view-the-database-optional"></a>Visualizzare il database (facoltativo)

Quando è stato eseguito il comando Update-database, EF ha creato il database e ha chiamato il metodo `Seed`. Quando si esegue l'applicazione in locale, EF usa il [database locale](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). È possibile visualizzare il database in Visual Studio. Dal menu **Visualizza** selezionare **Esplora oggetti di SQL Server**.

![](part-3/_static/image5.png)

Nella finestra di dialogo **Connetti al server** , nella casella di modifica **nome server** , digitare "(local DB) \v11.0". Lasciare l'opzione di **autenticazione** "autenticazione di Windows". Fare clic su **Connetti**.

![](part-3/_static/image6.png)

Visual Studio si connette al database locale e Visualizza i database esistenti nella finestra Esplora oggetti di SQL Server. È possibile espandere i nodi per visualizzare le tabelle create da EF.

![](part-3/_static/image7.png)

Per visualizzare i dati, fare clic con il pulsante destro del mouse su una tabella e selezionare **Visualizza dati**.

![](part-3/_static/image8.png)

La schermata seguente mostra i risultati della tabella books. Si noti che EF ha popolato il database con i dati di inizializzazione e la tabella contiene la chiave esterna per la tabella authors.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Precedente](part-2.md)
> [Successivo](part-4.md)
