---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Aggiunta di un nuovo campo al modello di filmato e alla tabella | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615087"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a>Aggiunta di un nuovo campo al modello di filmato e alla tabella

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.

In questa sezione si utilizzerà Migrazioni Code First di Entity Framework per eseguire la migrazione di alcune modifiche alle classi del modello in modo da applicare la modifica al database.

Per impostazione predefinita, quando si utilizza Entity Framework Code First per creare automaticamente un database, come in precedenza in questa esercitazione, Code First aggiunge una tabella al database per tenere traccia dell'eventuale sincronizzazione dello schema del database con le classi del modello da cui è stato generato. Se non sono sincronizzati, il Entity Framework genera un errore. In questo modo è più semplice tenere traccia dei problemi in fase di sviluppo che potrebbero essere individuati (per errori nascosti) in fase di esecuzione.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Impostazione Migrazioni Code First per le modifiche al modello

Se si usa Visual Studio 2012, fare doppio clic sul file *Movies. MDF* da Esplora soluzioni per aprire lo strumento database. Visual Studio Express per il Web mostrerà Esplora database, Visual Studio 2012 visualizzerà Esplora server. Se si usa Visual Studio 2010, usare Esplora oggetti di SQL Server.

Nello strumento database (Esplora database, Esplora server o Esplora oggetti di SQL Server), fare clic con il pulsante destro del mouse su `MovieDBContext` e selezionare **Elimina** per eliminare il database Movies.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Tornare a Esplora soluzioni. Fare clic con il pulsante destro del mouse sul file *Movies. MDF* e selezionare **Elimina** per rimuovere il database Movies.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Compilare l'applicazione e verificare che non siano presenti errori.

Nel menu **Strumenti** fare clic su **Gestione pacchetti NuGet** e quindi su **Console di Gestione pacchetti**.

![Aggiungi Pack Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

Nella finestra **console di gestione pacchetti** al prompt dei `PM>` immettere "Enable-Migrations-ContextTypeName MvcMovie. Models. MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

Il comando **Enable-Migrations** (illustrato in precedenza) consente di creare un file *Configuration.cs* in una nuova cartella *Migrations* .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio apre il file *Configuration.cs* . Sostituire il metodo `Seed` nel file *Configuration.cs* con il codice seguente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Fare clic con il pulsante destro del mouse sulla riga rossa ondulata sotto `Movie` e selezionare **Risolvi** e quindi **usare** **MvcMovie. Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Questa operazione consente di aggiungere l'istruzione using seguente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Migrazioni Code First chiama il metodo `Seed` dopo ogni migrazione (ovvero, chiamando **Update-database** nella console di gestione pacchetti) e questo metodo aggiorna le righe che sono già state inserite oppure le inserisce se non esistono ancora.

**Premere CTRL + MAIUSC + B per compilare il progetto.** I passaggi seguenti avranno esito negativo se non si compila a questo punto.

Il passaggio successivo consiste nel creare una classe `DbMigration` per la migrazione iniziale. Questa migrazione a crea un nuovo database, motivo per cui è stato eliminato il file *Movie. MDF* in un passaggio precedente.

Nella finestra **console di gestione pacchetti** immettere il comando "Aggiungi-migrazione iniziale" per creare la migrazione iniziale. Il nome "Initial" è arbitrario e viene usato per assegnare un nome al file di migrazione creato.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migrazioni Code First crea un altro file di classe nella cartella *Migrations* (denominato *{DateStamp}\_Initial.cs* ) e questa classe contiene il codice che crea lo schema del database. Il nome del file di migrazione è preceduto da un timestamp per facilitare l'ordinamento. Esaminare il file *{datestamp}\_Initial.cs* , che contiene le istruzioni per creare la tabella Movies per il database di film. Quando si aggiorna il database nelle istruzioni seguenti, questo file di *{datestamp}\_Initial.cs* verrà eseguito e creerà lo schema del database. Viene quindi eseguito il metodo **Seed** per popolare il database con i dati di test.

Nella **console di gestione pacchetti**immettere il comando "Update-database" per creare il database ed eseguire il metodo **Seed** .

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Se viene generato un errore che indica che una tabella esiste già e non può essere creata, è probabile che l'applicazione sia stata eseguita dopo l'eliminazione del database e prima dell'esecuzione `update-database`. In tal caso, eliminare nuovamente il file *Movies. MDF* , quindi riprovare a `update-database` comando. Se viene comunque ricevuto un errore, eliminare la cartella migrazioni e il contenuto, quindi iniziare con le istruzioni nella parte superiore di questa pagina, ovvero eliminare il file *Movies. MDF* , quindi passare a Enable-Migrations.

Eseguire l'applicazione e passare all'URL */Movies* . Vengono visualizzati i dati di inizializzazione.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Per iniziare, aggiungere una nuova proprietà `Rating` alla classe `Movie` esistente. Aprire il file *Models\Movie.cs* e aggiungere la proprietà `Rating` come questa:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

La classe `Movie` completa ora è simile al codice seguente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Compilare l'applicazione usando il comando di menu **compila** &gt;**Compila film** oppure premendo CTRL + MAIUSC + B.

Ora che è stata aggiornata la classe `Model`, è necessario aggiornare anche i modelli di visualizzazione *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* per visualizzare la nuova proprietà `Rating` nella visualizzazione del browser.

Aprire il file<em>\Views\Movies\Index.cshtml</em> e aggiungere un `<th>Rating</th>` intestazione di colonna immediatamente dopo la colonna <strong>Price</strong> . Aggiungere quindi una colonna `<td>` alla fine del modello per eseguire il rendering del valore `@item.Rating`. Di seguito è riportato il modello di visualizzazione <em>index. cshtml</em> aggiornato:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Successivamente, aprire il file *\Views\Movies\Create.cshtml* e aggiungere il markup seguente alla fine del modulo. Viene eseguito il rendering di una casella di testo in modo da poter specificare una classificazione quando viene creato un nuovo film.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova proprietà `Rating`.

A questo punto, eseguire l'applicazione e passare all'URL */Movies* . Quando si esegue questa operazione, tuttavia, verrà visualizzato uno degli errori seguenti:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Questo errore viene visualizzato perché la classe del modello di `Movie` aggiornata nell'applicazione è ora diversa dallo schema della tabella `Movie` del database esistente. Nella tabella del database non è presente una colonna `Rating`.

Per correggere questo errore, esistono alcuni approcci:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile quando si esegue lo sviluppo attivo su un database di prova. consente di sviluppare rapidamente lo schema del modello e del database insieme. Tuttavia, il lato negativo è che si perdono i dati esistenti nel database, quindi *non* si vuole usare questo approccio in un database di produzione. L'utilizzo di un inizializzatore per eseguire automaticamente il seeding di un database con dati di test è spesso un modo produttivo per sviluppare un'applicazione. Per altre informazioni sugli inizializzatori di database Entity Framework, vedere l' [esercitazione ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)di Tom Dykstra.
2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.
3. Usare Migrazioni Code First per aggiornare lo schema del database.

Per questa esercitazione si userà Migrazioni Code First.

Aggiornare il metodo seed in modo da fornire un valore per la nuova colonna. Aprire il file Migrations\Configuration.cs e aggiungere un campo rating a ogni oggetto Movie.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Compilare la soluzione, quindi aprire la finestra **console di gestione pacchetti** e immettere il comando seguente:

`add-migration AddRatingMig`

Il `add-migration` comando indica al Framework di migrazione di esaminare il modello di film corrente con lo schema del database del film corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello. AddRatingMig è arbitrario e viene usato per assegnare un nome al file di migrazione. È utile usare un nome significativo per il passaggio di migrazione.

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova classe derivata `DbMigration` e nel metodo `Up` è possibile visualizzare il codice che crea la nuova colonna.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Compilare la soluzione, quindi immettere il comando "Update-database" nella finestra **console di gestione pacchetti** .

Nell'immagine seguente viene illustrato l'output nella finestra della **console di gestione pacchetti** (il timbro di data anteposto AddRatingMig sarà diverso).

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Eseguire di nuovo l'applicazione e passare all'URL/Movies. È possibile visualizzare il nuovo campo rating.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Fare clic sul collegamento **Crea nuovo** per aggiungere un nuovo film. Si noti che è possibile aggiungere una classificazione.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Scegliere **Crea**. Il nuovo film, incluso il rating, viene ora visualizzato nell'elenco dei film:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

È anche necessario aggiungere il campo `Rating` ai modelli di vista edit, Details e SearchIndex.

È possibile immettere di nuovo il comando "Update-database" nella finestra della **console di gestione pacchetti** e non verrà apportata alcuna modifica, perché lo schema corrisponde al modello.

In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche. È stato inoltre illustrato un modo per popolare un database appena creato con dati di esempio, in modo da poter provare gli scenari. Si osserverà ora come aggiungere una logica di convalida più completa alle classi del modello e abilitare l'applicazione di alcune regole business.

> [!div class="step-by-step"]
> [Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)
