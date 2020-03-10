---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: distribuzione di un aggiornamento del database-9 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 3385e1019d9e7a9263fd9513c39b25eb85febbb5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564442"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: distribuzione di un aggiornamento del database-9 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in app Azure app Web del servizio, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Panoramica

In questa esercitazione si apportano modifiche al database e modifiche del codice correlate, si verificano le modifiche in Visual Studio e quindi si distribuisce l'aggiornamento negli ambienti di test e di produzione.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Aggiunta di una nuova colonna a una tabella

In questa sezione viene aggiunta una colonna della data di nascita alla classe di base `Person` per le entità `Student` e `Instructor`. Si aggiorna quindi la pagina in cui vengono visualizzati i dati dell'insegnante, in modo che venga visualizzata la nuova colonna.

Nel progetto *ContosoUniversity. dal* aprire *Person.cs* e aggiungere la proprietà seguente alla fine della classe `Person` (sono presenti due parentesi graffe di chiusura successive):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Successivamente, aggiornare il metodo seed in modo da fornire un valore per la nuova colonna. Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con il blocco di codice seguente, che include informazioni sulla data di nascita:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

Nel progetto ContosoUniversity aprire *Instructors. aspx* e aggiungere un nuovo campo modello per visualizzare la data di nascita. Aggiungerlo tra quelli per data di assunzione e assegnazione dell'ufficio:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

Se il rientro del codice non viene sincronizzato, è possibile premere CTRL + K, quindi CTRL + D per riformattare automaticamente il file.

Compilare la soluzione, quindi aprire la finestra **console di gestione pacchetti** . Assicurarsi che ContosoUniversity. DAL sia ancora selezionato come **progetto predefinito**.

Nella finestra **console di gestione pacchetti** selezionare **ContosoUniversity. dal** come **progetto predefinito**, quindi immettere il comando seguente:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova classe `DbMigration` e nel metodo `Up` è possibile visualizzare il codice che crea la nuova colonna.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Compilare la soluzione, quindi immettere il comando seguente nella finestra della **console di gestione pacchetti** (assicurarsi che il progetto CONTOSOUNIVERSITY. dal sia ancora selezionato):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Al termine del comando, eseguire l'applicazione e selezionare la pagina Instructors (insegnanti). Quando la pagina viene caricata, si noterà che contiene il nuovo campo Data di nascita.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Distribuzione dell'aggiornamento del database nell'ambiente di test

In **Esplora soluzioni** selezionare il progetto ContosoUniversity.

Nel **Web fare clic su pubblica** barra degli strumenti, selezionare il profilo di pubblicazione del **test** , quindi fare clic su **Pubblica sito Web**. Se la barra degli strumenti è disabilitata, selezionare il progetto ContosoUniversity in **Esplora soluzioni**.

Visual Studio distribuisce l'applicazione aggiornata e il browser si apre al home page. Eseguire la pagina insegnanti per verificare che l'aggiornamento sia stato distribuito correttamente. Quando l'applicazione tenta di accedere al database per questa pagina, Code First aggiorna lo schema del database ed esegue il `Seed` metodo. Quando viene visualizzata la pagina, viene visualizzata la colonna della **Data di nascita** prevista con le date al suo interno.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Distribuzione dell'aggiornamento del database nell'ambiente di produzione

È ora possibile eseguire la distribuzione nell'ambiente di produzione. L'unica differenza è che si userà *app\_offline. htm* per impedire agli utenti di accedere al sito e quindi aggiornare il database durante la distribuzione delle modifiche. Per la distribuzione di produzione, seguire questa procedura:

- Caricare l' *app\_file offline. htm* nel sito di produzione.
- In Visual Studio scegliere il profilo di produzione nella barra degli strumenti di **pubblicazione sul Web** , quindi fare clic su **Pubblica sito Web**.
- Eliminare l' *app\_file offline. htm* dal sito di produzione.

> [!NOTE]
> Mentre l'applicazione è in uso nell'ambiente di produzione, è necessario implementare un piano di backup. Ovvero, è necessario copiare periodicamente i file *School-prod. sdf* e *ASPNET-prod. sdf* dal sito di produzione in un percorso di archiviazione protetto ed è necessario mantenere diverse generazioni di tali backup. Quando si aggiorna il database, è consigliabile eseguire una copia di backup immediatamente prima della modifica. Quindi, se si commette un errore e non lo si rileva fino a quando non viene distribuito nell'ambiente di produzione, sarà comunque possibile ripristinare il database allo stato in cui si trovava prima che venisse danneggiato.

Quando Visual Studio apre l'URL del home page nel browser, viene visualizzata la pagina *app\_offline. htm* . Dopo aver eliminato l' *app\_file offline. htm* , è possibile passare nuovamente al Home page per verificare che l'aggiornamento sia stato distribuito correttamente.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

A questo punto è stato distribuito un aggiornamento dell'applicazione che includeva una modifica del database sia per i test che per la produzione. L'esercitazione successiva illustra come eseguire la migrazione del database da SQL Server Compact a SQL Server Express e SQL Server.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
