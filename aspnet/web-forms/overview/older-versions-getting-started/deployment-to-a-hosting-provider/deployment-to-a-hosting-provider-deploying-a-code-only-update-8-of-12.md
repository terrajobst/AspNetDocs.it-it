---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: distribuzione di un aggiornamento solo codice-8 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: e4d094ef84a747c36ce05ddb0e3d1ce0391d5605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564407"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di solo codice-8 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in app Azure app Web del servizio, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Panoramica

Dopo la distribuzione iniziale, il lavoro di manutenzione e sviluppo del sito Web continua e, prima di lungo, sarà necessario distribuire un aggiornamento. Questa esercitazione illustra il processo di distribuzione di un aggiornamento nel codice dell'applicazione. Questo aggiornamento non implica una modifica del database. verranno visualizzate le differenze relative alla distribuzione di una modifica del database nell'esercitazione successiva.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Apportare una modifica al codice

Come semplice esempio di aggiornamento dell'applicazione, si aggiungerà alla pagina degli **insegnanti** un elenco di corsi insegnato dall'insegnante selezionato.

Se si esegue la pagina **Instructors (insegnanti** ), si noterà che nella griglia sono presenti collegamenti **selezionati** , ma che non eseguono alcuna operazione oltre a rendere grigio lo sfondo della riga.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

A questo punto si aggiungerà il codice che viene eseguito quando si fa clic sul collegamento **Seleziona** e viene visualizzato un elenco dei corsi insegnati dall'insegnante selezionato.

In *Instructors. aspx*aggiungere il markup seguente subito dopo il controllo `Label` **ErrorMessageLabel** :

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Eseguire la pagina e selezionare un insegnante. Viene visualizzato un elenco dei corsi tenuti da tale insegnante.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Distribuzione dell'aggiornamento del codice nell'ambiente di test

La distribuzione nell'ambiente di test è una semplice operazione di esecuzione di un solo clic di pubblicazione. Per rendere più rapido questo processo, è possibile usare la barra degli strumenti **pubblica con un clic** .

Scegliere **barre degli strumenti** dal menu **Visualizza** e quindi **fare clic sul pulsante pubblica**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

In **Esplora soluzioni**selezionare il progetto ContosoUniversity.

sul **Web fare clic su pubblica** barra degli strumenti, scegliere il profilo di pubblicazione del **test** , quindi fare clic su **Pubblica Web** (icona con frecce che puntano a sinistra e a destra).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio distribuisce l'applicazione aggiornata e il browser si apre automaticamente al home page. Eseguire la pagina insegnanti e selezionare un insegnante per verificare che l'aggiornamento sia stato distribuito correttamente.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Normalmente si eseguono anche test di regressione, ovvero si testa il resto del sito per assicurarsi che la nuova modifica non interrompa le funzionalità esistenti. Tuttavia, per questa esercitazione si ignorerà questo passaggio e si procederà con la distribuzione dell'aggiornamento nell'ambiente di produzione.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Prevenzione della ridistribuzione dello stato iniziale del database nell'ambiente di produzione

In un'applicazione reale, gli utenti interagiscono con il sito di produzione dopo la distribuzione iniziale e i database vengono popolati con dati dinamici. Pertanto, non si vuole ridistribuire il database delle appartenenze nello stato iniziale, eliminando tutti i dati attivi. Poiché i database SQL Server Compact sono file nella cartella *app\_data* , è necessario evitare questo problema cambiando le impostazioni di distribuzione in modo che i file nella cartella *app\_data* non vengano distribuiti.

Aprire la finestra delle **proprietà del progetto** per il progetto ContosoUniversity e selezionare la scheda **pacchetto/pubblica Web** . Assicurarsi che nella casella di riepilogo a discesa **configurazione** sia selezionata l'opzione **attiva (versione)** o **versione** selezionare **Escludi file dalla cartella app\_data**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Se si decide di distribuire una build di debug in futuro, è consigliabile apportare la stessa modifica per la configurazione della build di debug: modificare la **configurazione** per eseguire il **debug** e quindi selezionare **Escludi file dalla cartella app\_data**.

Salvare e chiudere la scheda **pubblicazione/pubblicazione del pacchetto Web** .

> [!NOTE] 
> 
> [!IMPORTANT]
> Assicurarsi di non avere **rimosso i file aggiuntivi nella destinazione** selezionata nei profili di pubblicazione. Se si seleziona questa opzione, il processo di distribuzione eliminerà i database presenti nei dati dell'app\_nel sito distribuito e eliminerà l'app\_cartella dati.

## <a name="preventing-user-access-to-the-production-site-during-update"></a>Impedire l'accesso degli utenti al sito di produzione durante l'aggiornamento

La modifica che si sta distribuendo ora è una semplice modifica a una singola pagina. Tuttavia, a volte si distribuiscono modifiche di dimensioni maggiori e, in tal caso, il sito può comportarsi in modo anomalo se un utente richiede una pagina prima del completamento della distribuzione. Per evitare questo problema, è possibile usare un' *app\_file offline. htm* . Quando si inserisce un file denominato *app\_offline. htm* nella cartella radice dell'applicazione, in IIS viene visualizzato automaticamente tale file anziché eseguire l'applicazione. Per impedire l'accesso durante la distribuzione, inserire l' *app\_offline. htm* nella cartella radice, eseguire il processo di distribuzione e quindi rimuovere l' *app\_offline. htm*.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla soluzione, non su uno dei progetti, e selezionare **nuova cartella soluzione**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Denominare la cartella *SolutionFiles*.

Nella nuova cartella creare una pagina HTML denominata *app\_offline. htm*. Sostituire il contenuto esistente con il markup seguente:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

È possibile copiare l' *app\_file offline. htm* nel sito usando una connessione FTP o l'utilità **file Manager** nel pannello di controllo del provider di hosting. Per questa esercitazione si userà **Gestione file**.

Aprire il pannello di controllo e selezionare **file Manager** come nell'esercitazione [distribuzione in ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . Selezionare **contosouniversity.com** e quindi **wwwroot** per ottenere la cartella radice dell'applicazione e quindi fare clic su **carica**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

Nella finestra di dialogo **Carica file** selezionare l' *app\_file offline. htm* , quindi fare clic su **carica**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Passare all'URL del proprio sito. Si noterà che viene visualizzata la pagina *app\_offline. htm* invece della Home page.

[![App_offline. htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

A questo punto si è pronti per la distribuzione nell'ambiente di produzione.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Distribuzione dell'aggiornamento del codice nell'ambiente di produzione

Nella barra degli strumenti **pubblica sul Web fare clic** sul profilo di pubblicazione di **produzione** , quindi scegliere **Pubblica sito Web**.

Visual Studio distribuisce l'applicazione aggiornata e apre il browser all'home page del sito. Viene visualizzata l' *app\_file offline. htm* . Prima di poter eseguire il test per verificare la corretta distribuzione, è necessario rimuovere l' *app\_file offline. htm* .

Tornare all'applicazione **file Manager** nel pannello di controllo. Selezionare **contosouniversity.com** e **wwwroot**, selezionare **app\_offline. htm**, quindi fare clic su **Elimina**.

[![Deleting_app_offline. htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

Nel browser aprire la pagina insegnanti nel sito pubblico e selezionare un insegnante per verificare che l'aggiornamento sia stato distribuito correttamente.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

A questo punto è stato distribuito un aggiornamento dell'applicazione che non implicava una modifica del database. L'esercitazione successiva illustra come distribuire una modifica del database.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
