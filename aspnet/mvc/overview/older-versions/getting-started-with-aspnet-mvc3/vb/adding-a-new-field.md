---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Aggiunta di un nuovo campo al modello di film e alla tabella di database (VB) | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: b2b26b6009c55f02c8a4159bda839fe7aefea4c0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540579"
---
# <a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Aggiunta di un nuovo campo al modello di filmato e alla tabella di database (VB)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, è disponibile un progetto Visual Web Developer con codice sorgente VB.NET. [Scaricare la versione di VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce C#, passare alla [ C# versione](../cs/adding-a-new-field.md) di questa esercitazione.

In questa sezione verranno apportate alcune modifiche alle classi del modello e verrà illustrato come aggiornare lo schema del database in base alle modifiche apportate al modello.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Per iniziare, aggiungere una nuova proprietà `Rating` alla classe `Movie` esistente. Aprire il file *Movie.cs* e aggiungere la proprietà `Rating` come questa:

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

La classe `Movie` completa ora è simile al codice seguente:

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

Ricompilare l'applicazione utilizzando il comando di menu **Debug** &gt;**Compila filmato** .

Ora che è stata aggiornata la classe `Model`, è necessario aggiornare anche i modelli di visualizzazione *\Views\Movies\Index.vbhtml* e *\Views\Movies\Create.vbhtml* per supportare la nuova proprietà `Rating`.

Aprire il file<em>\Views\Movies\Index.vbhtml</em> e aggiungere un `<th>Rating</th>` intestazione di colonna immediatamente dopo la colonna <strong>Price</strong> . Aggiungere quindi una colonna `<td>` alla fine del modello per eseguire il rendering del valore `@item.Rating`. Di seguito è riportato il modello di visualizzazione <em>index. vbhtml</em> aggiornato:

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

Successivamente, aprire il file *\Views\Movies\Create.vbhtml* e aggiungere il markup seguente alla fine del modulo. Viene eseguito il rendering di una casella di testo in modo da poter specificare una classificazione quando viene creato un nuovo film.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Gestione delle differenze dello schema del modello e del database

A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova proprietà `Rating`.

A questo punto, eseguire l'applicazione e passare all'URL */Movies* . Quando si esegue questa operazione, tuttavia, verrà visualizzato l'errore seguente:

![](adding-a-new-field/_static/image1.png)

Questo errore viene visualizzato perché la classe del modello di `Movie` aggiornata nell'applicazione è ora diversa dallo schema della tabella `Movie` del database esistente. Nella tabella del database non è presente una colonna `Rating`.

Per impostazione predefinita, quando si utilizza Entity Framework Code First per creare automaticamente un database, come in precedenza in questa esercitazione, Code First aggiunge una tabella al database per tenere traccia dell'eventuale sincronizzazione dello schema del database con le classi del modello da cui è stato generato. Se non sono sincronizzati, il Entity Framework genera un errore. In questo modo è più semplice tenere traccia dei problemi in fase di sviluppo che potrebbero essere individuati (per errori nascosti) in fase di esecuzione. La funzionalità di controllo della sincronizzazione consente di visualizzare il messaggio di errore appena visto.

Esistono due approcci per risolvere l'errore:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile quando si esegue lo sviluppo attivo su un database di test, poiché consente di sviluppare rapidamente lo schema del modello e del database insieme. Tuttavia, il lato negativo è che si perdono i dati esistenti nel database, quindi *non* si vuole usare questo approccio in un database di produzione.
2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.

Per questa esercitazione, verrà usato il primo approccio: il Entity Framework Code First ricreare automaticamente il database ogni volta che il modello viene modificato.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Ricreare automaticamente il database in base alle modifiche del modello

Verrà ora aggiornata l'applicazione in modo che Code First elimini automaticamente e ricrei il database ogni volta che si modifica il modello per l'applicazione.

> [!NOTE] 
> 
> **Avviso** di È necessario abilitare questo approccio per eliminare e ricreare automaticamente il database solo quando si utilizza un database di sviluppo o di test e *mai* in un database di produzione che contiene dati reali. L'utilizzo in un server di produzione può causare la perdita di dati.

In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi selezionare **classe**.

![](adding-a-new-field/_static/image2.png)

Denominare la classe &quot;&quot;MovieInitializer. Aggiornare la classe `MovieInitializer` in modo che contenga il codice seguente:

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

La classe `MovieInitializer` specifica che il database utilizzato dal modello deve essere eliminato e ricreato automaticamente se le classi del modello cambiano. Il codice include un metodo di `Seed` per specificare alcuni dati predefiniti da aggiungere automaticamente al database ogni volta che viene creata o ricreata. Si tratta di un modo utile per popolare il database con alcuni dati di esempio, senza che sia necessario popolarlo manualmente ogni volta che viene apportata una modifica al modello.

Ora che è stata definita la classe `MovieInitializer`, sarà necessario collegarla in modo che ogni volta che viene eseguita l'applicazione, verifichi se le classi del modello sono diverse dallo schema nel database. In tal caso, è possibile eseguire l'inizializzatore per ricreare il database in modo che corrisponda al modello e quindi popolare il database con i dati di esempio.

Aprire il file *Global. asax* alla radice del progetto `MvcMovies`:

Il file *Global. asax* contiene la classe che definisce l'intera applicazione per il progetto e contiene un gestore eventi `Application_Start` che viene eseguito all'avvio iniziale dell'applicazione.

Trovare il metodo `Application_Start` e aggiungere una chiamata a `Database.SetInitializer` all'inizio del metodo, come illustrato di seguito:

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

L'istruzione `Database.SetInitializer` appena aggiunta indica che il database utilizzato dall'istanza di `MovieDBContext` deve essere eliminato e ricreato automaticamente se lo schema e il database non corrispondono. Come si è visto, il database verrà inoltre popolato con i dati di esempio specificati nella classe `MovieInitializer`.

Chiudere il file *Global. asax* .

Eseguire di nuovo l'applicazione e passare all'URL */Movies* . All'avvio dell'applicazione, viene rilevato che la struttura del modello non corrisponde più allo schema del database. Ricrea automaticamente il database in modo che corrisponda alla nuova struttura del modello e popola il database con i film di esempio:

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

Fare clic sul collegamento **Crea nuovo** per aggiungere un nuovo film. Si noti che è possibile aggiungere una classificazione.

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

Scegliere **Crea**. Il nuovo film, incluso il rating, viene ora visualizzato nell'elenco dei film:

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche. È stato inoltre illustrato un modo per popolare un database appena creato con dati di esempio, in modo da poter provare gli scenari. Si osserverà ora come aggiungere una logica di convalida più completa alle classi del modello e abilitare l'applicazione di alcune regole business.

> [!div class="step-by-step"]
> [Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)
