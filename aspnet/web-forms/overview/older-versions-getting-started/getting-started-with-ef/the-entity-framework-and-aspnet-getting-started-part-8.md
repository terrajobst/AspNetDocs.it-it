---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Form-parte 8 | Microsoft Docs
author: tdykstra
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: ff6a808dd501df8056c0fb0784a8e42e094d5db7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585911"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Introduzione con Entity Framework 4,0 Database First e ASP.NET 4 Web Forms-parte 8

di [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4,0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Uso della funzionalità Dynamic Data per formattare e convalidare i dati

Nell'esercitazione precedente sono state implementate le stored procedure. In questa esercitazione viene illustrato come Dynamic Data funzionalità può offrire i vantaggi seguenti:

- I campi vengono formattati automaticamente per la visualizzazione in base al relativo tipo di dati.
- I campi vengono convalidati automaticamente in base al relativo tipo di dati.
- È possibile aggiungere metadati al modello di dati per personalizzare il comportamento di formattazione e convalida. Quando si esegue questa operazione, è possibile aggiungere le regole di formattazione e convalida in una sola posizione, che vengono applicate automaticamente ovunque si acceda ai campi utilizzando Dynamic Data controlli.

Per verificarne il funzionamento, verranno modificati i controlli usati per visualizzare e modificare i campi nella pagina *students. aspx* esistente e verranno aggiunti i metadati di formattazione e di convalida ai campi nome e data del tipo di entità `Student`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Uso di controlli DynamicField e DynamicControl

Aprire la pagina *students. aspx* e nel controllo `StudentsGridView` sostituire il **nome** e la **Data di registrazione** `TemplateField` elementi con il markup seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Questo markup usa controlli `DynamicControl` al posto dei controlli `TextBox` e `Label` nel campo modello nome studente e usa un controllo `DynamicField` per la data di registrazione. Nessuna stringa di formato specificata.

Aggiungere un controllo `ValidationSummary` dopo il controllo `StudentsGridView`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

Nel controllo `SearchGridView` sostituire il markup per le colonne **nome** e **Data di registrazione** come nel controllo `StudentsGridView`, tranne omettere l'elemento `EditItemTemplate`. L'elemento `Columns` del controllo `SearchGridView` ora contiene il markup seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Aprire *students.aspx.cs* e aggiungere l'istruzione `using` seguente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Aggiungere un gestore per l'evento `Init` della pagina:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Questo codice specifica che Dynamic Data fornirà la formattazione e la convalida in questi controlli associati a dati per i campi dell'entità `Student`. Se viene ricevuto un messaggio di errore simile all'esempio seguente quando si esegue la pagina, in genere significa che è stato dimenticato di chiamare il metodo `EnableDynamicData` in `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Eseguire la pagina.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

Nella colonna **Data di registrazione** l'ora viene visualizzata insieme alla data, perché il tipo di proprietà è `DateTime`. Questo problema verrà risolto in un secondo momento.

Per ora, si noti che Dynamic Data fornisce automaticamente la convalida dei dati di base. Ad esempio, fare clic su **modifica**, deselezionare il campo data, fare clic su **Aggiorna**. si noterà che Dynamic Data rende automaticamente questo campo obbligatorio perché il valore non ammette i valori null nel modello di dati. La pagina Visualizza un asterisco dopo il campo e un messaggio di errore nel controllo `ValidationSummary`:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

È possibile omettere il controllo `ValidationSummary`, perché è anche possibile mantenere il puntatore del mouse sull'asterisco per visualizzare il messaggio di errore:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dynamic Data convaliderà anche che i dati immessi nel campo **Data di registrazione** è una data valida:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Come si può notare, si tratta di un messaggio di errore generico. Nella sezione successiva si vedrà come personalizzare i messaggi, nonché le regole di convalida e formattazione.

## <a name="adding-metadata-to-the-data-model"></a>Aggiunta di metadati al modello di dati

In genere, è necessario personalizzare la funzionalità fornita da Dynamic Data. Ad esempio, è possibile modificare la modalità di visualizzazione dei dati e il contenuto dei messaggi di errore. Le regole di convalida dei dati vengono in genere personalizzate anche per offrire più funzionalità rispetto a quelle fornite Dynamic Data automaticamente in base ai tipi di dati. A tale scopo, creare classi parziali che corrispondono ai tipi di entità.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **ContosoUniversity** , scegliere **Aggiungi riferimento**e aggiungere un riferimento a `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

Nella cartella *dal* creare un nuovo file di classe, denominarlo *Student.cs*e sostituire il codice del modello con il codice seguente.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Questo codice crea una classe parziale per l'entità `Student`. L'attributo `MetadataType` applicato a questa classe parziale identifica la classe che si sta utilizzando per specificare i metadati. La classe di metadati può avere qualsiasi nome, ma l'uso del nome di entità più "Metadata" è una pratica comune.

Gli attributi applicati alle proprietà nella classe di metadati specificano la formattazione, la convalida, le regole e i messaggi di errore. Gli attributi illustrati di seguito avranno i risultati seguenti:

- `EnrollmentDate` viene visualizzata come data (senza l'ora).
- Entrambi i campi nome devono avere una lunghezza inferiore a 25 caratteri e viene fornito un messaggio di errore personalizzato.
- Sono necessari entrambi i campi nome e viene fornito un messaggio di errore personalizzato.

Eseguire di nuovo la pagina *students. aspx.* si noterà che le date sono ora visualizzate senza orari:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Modificare una riga e provare a cancellare i valori nei campi del nome. Gli asterischi che indicano gli errori dei campi vengono visualizzati non appena si lascia un campo, prima di fare clic su **Aggiorna**. Quando si fa clic su **Aggiorna**, nella pagina viene visualizzato il testo del messaggio di errore specificato.

[![image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Provare a immettere nomi con una lunghezza superiore a 25 caratteri, fare clic su **Aggiorna**e nella pagina viene visualizzato il testo del messaggio di errore specificato.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Ora che sono state configurate queste regole di formattazione e di convalida nei metadati del modello di dati, le regole verranno applicate automaticamente in ogni pagina in cui vengono visualizzate o consentite le modifiche a questi campi, purché si utilizzino controlli `DynamicControl` o `DynamicField`. In questo modo si riduce la quantità di codice ridondante da scrivere, rendendo più semplice la programmazione e il testing e si garantisce che la formattazione e la convalida dei dati siano coerenti in un'applicazione.

## <a name="more-information"></a>Altre informazioni

Questa serie di esercitazioni su Introduzione con il Entity Framework. Per altre risorse utili per imparare a usare la Entity Framework, continuare con [la prima esercitazione della serie di esercitazioni Entity Framework successive](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) oppure visitare i siti seguenti:

- [Domande frequenti Entity Framework](http://www.ef-faq.org/introduction.html)
- [Blog del team di Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework in MSDN Library](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework in MSDN Data Developer Center](https://msdn.microsoft.com/data/ef.aspx)
- [Cenni preliminari sul controllo server Web EntityDataSource in MSDN Library](https://msdn.microsoft.com/library/cc488502.aspx)
- [Informazioni di riferimento sull'API di controllo EntityDataSource in MSDN Library](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Forum Entity Framework su MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog di Julie Lerman](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-7.md)
