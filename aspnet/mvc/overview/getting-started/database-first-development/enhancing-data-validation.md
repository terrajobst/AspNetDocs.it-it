---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: "Esercitazione: migliorare la convalida dei dati per Database First EF con l'app MVC ASP.NET"
description: Questa esercitazione è incentrata sull'aggiunta di annotazioni di dati al modello di dati per specificare i requisiti di convalida e la formattazione della visualizzazione.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616277"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>Esercitazione: migliorare la convalida dei dati per Database First EF con l'app MVC ASP.NET

Usando l'impalcatura MVC, Entity Framework e ASP.NET, è possibile creare un'applicazione Web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare automaticamente il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database.

Questa esercitazione è incentrata sull'aggiunta di annotazioni di dati al modello di dati per specificare i requisiti di convalida e la formattazione della visualizzazione. È stato migliorato in base ai commenti e suggerimenti degli utenti nella sezione dei commenti.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungi annotazioni dati
> * Aggiungi classi di metadati

## <a name="prerequisites"></a>Prerequisiti

* [Personalizzare una vista](customizing-a-view.md)

## <a name="add-data-annotations"></a>Aggiungi annotazioni dati

Come si è visto in un argomento precedente, alcune regole di convalida dei dati vengono applicate automaticamente all'input dell'utente. Ad esempio, è possibile specificare solo un numero per la proprietà Grade. Per specificare più regole di convalida dei dati, è possibile aggiungere annotazioni di dati alla classe del modello. Queste annotazioni vengono applicate nell'applicazione Web per la proprietà specificata. È anche possibile applicare attributi di formattazione che modificano la modalità di visualizzazione delle proprietà. ad esempio, modificando il valore utilizzato per le etichette di testo.

In questa esercitazione vengono aggiunte le annotazioni dei dati per limitare la lunghezza dei valori specificati per le proprietà FirstName, LastName e MiddleName. Nel database, questi valori sono limitati a 50 caratteri; nell'applicazione Web, tuttavia, non è attualmente applicato il limite di caratteri. Se un utente fornisce più di 50 caratteri per uno di questi valori, la pagina si arresterà in modo anomalo durante il tentativo di salvataggio del valore nel database. Si limiterà anche la classificazione dei valori compresi tra 0 e 4.

Selezionare **modelli** > **ContosoModel. edmx** > **ContosoModel.tt** e aprire il file *Student.cs* . Aggiungere il codice evidenziato seguente alla classe.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Aprire *Enrollment.cs* e aggiungere il codice evidenziato seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Compilare la soluzione.

Fare clic su **elenco di studenti** e selezionare **modifica**. Se si tenta di immettere più di 50 caratteri, viene visualizzato un messaggio di errore.

![Mostra messaggio di errore](enhancing-data-validation/_static/image1.png)

Tornare alla home page. Fare clic su **elenco di registrazioni** e selezionare **modifica**. Tentativo di fornire un livello superiore a 4. Verrà visualizzato questo errore: *il livello di campo deve essere compreso tra 0 e 4.*

## <a name="add-metadata-classes"></a>Aggiungi classi di metadati

L'aggiunta di attributi di convalida direttamente alla classe del modello funziona quando non si prevede che il database venga modificato. Tuttavia, se il database viene modificato ed è necessario rigenerare la classe del modello, si perderanno tutti gli attributi applicati alla classe del modello. Questo approccio può essere estremamente inefficiente e soggetto alla perdita di importanti regole di convalida.

Per evitare questo problema, è possibile aggiungere una classe di metadati che contiene gli attributi. Quando si associa la classe del modello alla classe di metadati, gli attributi vengono applicati al modello. In questo approccio, la classe del modello può essere rigenerata senza perdere tutti gli attributi applicati alla classe di metadati.

Nella cartella **Models** aggiungere una classe denominata *Metadata.cs*.

Sostituire il codice in *Metadata.cs* con il codice seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Queste classi di metadati contengono tutti gli attributi di convalida applicati in precedenza alle classi del modello. L'attributo **display** viene usato per modificare il valore usato per le etichette di testo.

A questo punto, è necessario associare le classi del modello alle classi di metadati.

Nella cartella **Models** aggiungere una classe denominata *PartialClasses.cs*.

Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Si noti che ogni classe è contrassegnata come classe `partial` e ogni classe corrisponde al nome e allo spazio dei nomi della classe generata automaticamente. Applicando l'attributo Metadata alla classe parziale, è necessario assicurarsi che gli attributi di convalida dei dati vengano applicati alla classe generata automaticamente. Questi attributi non andranno perduti quando si rigenerano le classi del modello perché l'attributo dei metadati viene applicato nelle classi parziali che non vengono rigenerate.

Per rigenerare le classi generate automaticamente, aprire il file *ContosoModel. edmx* . Ancora una volta, fare clic con il pulsante destro del mouse sull'area di progettazione e scegliere **Aggiorna modello da database**. Anche se il database non è stato modificato, questo processo rigenererà le classi. Nella scheda **Aggiorna** selezionare **tabelle** e **fine**.

Salvare il file *ContosoModel. edmx* per applicare le modifiche.

Aprire il file *Student.cs* o *Enrollment.cs* e notare che gli attributi di convalida dei dati applicati in precedenza non sono più nel file. Tuttavia, eseguire l'applicazione e notare che le regole di convalida vengono comunque applicate quando si immettono dati.

## <a name="conclusion"></a>Conclusione

Questa serie ha fornito un semplice esempio di come generare codice da un database esistente che consente agli utenti di modificare, aggiornare, creare ed eliminare dati. Per creare il progetto, è stato usato ASP.NET MVC 5, Entity Framework e l'impalcatura di ASP.NET. 

Per un esempio introduttivo di sviluppo Code First, vedere [Introduzione con ASP.NET MVC 5](../introduction/getting-started.md). 

Per un esempio più avanzato, vedere la pagina relativa alla [creazione di un modello di dati Entity Framework per un'App ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Si noti che l'API DbContext usata per lavorare con i dati in Database First è identica a quella usata per l'utilizzo dei dati in Code First. Anche se si intende usare Database First, è possibile imparare a gestire scenari più complessi, ad esempio la lettura e l'aggiornamento dei dati correlati, la gestione dei conflitti di concorrenza e così via da un'esercitazione Code First. L'unica differenza risiede nel modo in cui vengono create le classi database, context e Entity.

## <a name="additional-resources"></a>Risorse aggiuntive

Per un elenco completo delle annotazioni di convalida dei dati che è possibile applicare alle proprietà e alle classi, vedere [System. ComponentModel. DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Annotazioni dei dati aggiunte
> * Classi di metadati aggiunte

Per informazioni su come distribuire un'app Web e un database SQL in app Azure servizio, vedere questa esercitazione:
> [!div class="nextstepaction"]
> [Distribuire un'app .NET nel servizio app Azure](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
