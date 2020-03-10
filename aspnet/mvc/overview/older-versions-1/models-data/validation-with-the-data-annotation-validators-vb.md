---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Convalida con i validator di annotazione dei dati (VB) | Microsoft Docs
author: microsoft
description: Utilizzare lo strumento di associazione di modelli di annotazione dati per eseguire la convalida in un'applicazione MVC ASP.NET. Informazioni su come usare i diversi tipi di validator...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 00150575baabc659f7dd0c07349cde52105f6c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542084"
---
# <a name="validation-with-the-data-annotation-validators-vb"></a>Convalida con i validator di annotazione dei dati (VB)

[Microsoft](https://github.com/microsoft)

> Utilizzare lo strumento di associazione di modelli di annotazione dati per eseguire la convalida in un'applicazione MVC ASP.NET. Informazioni su come usare i diversi tipi di attributi validator e usarli in Microsoft Entity Framework.

In questa esercitazione si apprenderà come usare i validator di annotazione dei dati per eseguire la convalida in un'applicazione MVC ASP.NET. Il vantaggio di usare i validator di annotazione dei dati è che consentono di eseguire la convalida semplicemente aggiungendo uno o più attributi, ad esempio l'attributo Required o StringLength, a una proprietà della classe.

Prima di poter utilizzare i validator di annotazione dei dati, è necessario scaricare lo strumento di associazione di modelli per le annotazioni dei dati. È possibile scaricare l'esempio di strumento di associazione di modelli per le annotazioni dei dati dal sito Web CodePlex facendo clic [qui](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).

È importante comprendere che lo strumento di associazione di modelli per le annotazioni dei dati non è una parte ufficiale del Framework Microsoft ASP.NET MVC. Sebbene lo strumento di associazione di modelli di annotazioni dei dati sia stato creato dal team di Microsoft ASP.NET MVC, Microsoft non offre il supporto ufficiale per lo strumento di associazione di modelli per le annotazioni dei dati descritto e utilizzato in questa esercitazione.

## <a name="using-the-data-annotation-model-binder"></a>Utilizzo dello strumento di associazione di modelli di annotazione dati

Per usare lo strumento di associazione di modelli per le annotazioni dei dati in un'applicazione MVC ASP.NET, è innanzitutto necessario aggiungere un riferimento all'assembly Microsoft. Web. Mvc. DataAnnotations. dll e l'assembly System. ComponentModel. DataAnnotations. dll. Selezionare l'opzione di menu **progetto, Aggiungi riferimento**. Fare quindi clic sulla scheda **Sfoglia** e passare al percorso in cui è stato scaricato (e decompresso) l'esempio di strumento di associazione di modelli per le annotazioni dei dati (vedere la **Figura 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figura 1**: aggiunta di un riferimento allo strumento di associazione di modelli per le annotazioni dei dati ([fare clic per visualizzare l'immagine con dimensioni complete](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Selezionare l'assembly Microsoft. Web. Mvc. DataAnnotations. dll e l'assembly System. ComponentModel. DataAnnotations. dll e fare clic sul pulsante **OK** .

Non è possibile utilizzare l'assembly System. ComponentModel. DataAnnotations. dll incluso in .NET Framework Service Pack 1 con lo strumento di associazione di modelli per le annotazioni dei dati. È necessario usare la versione dell'assembly System. ComponentModel. DataAnnotations. dll incluso con il download dell'esempio di strumento di associazione di modelli di annotazioni dei dati.

Infine, è necessario registrare il gestore di associazione del modello DataAnnotations nel file Global. asax. Aggiungere la riga di codice seguente al gestore eventi dell'applicazione\_Start () in modo che il metodo dell'applicazione\_Start () sia simile al seguente:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Questa riga di codice registra DataAnnotationsModelBinder come strumento di associazione di modelli predefinito per l'intera applicazione MVC ASP.NET.

## <a name="using-the-data-annotation-validator-attributes"></a>Utilizzo degli attributi del validator di annotazione dei dati

Quando si utilizza lo strumento di associazione di modelli di annotazioni dati, per eseguire la convalida vengono utilizzati attributi validator. Lo spazio dei nomi System. ComponentModel. DataAnnotations include gli attributi validator seguenti:

- Intervallo: consente di verificare se il valore di una proprietà è compreso tra un intervallo di valori specificato.
- RegularExpression: consente di verificare se il valore di una proprietà corrisponde a un criterio di espressione regolare specificato.
- Obbligatorio: consente di contrassegnare una proprietà come obbligatoria.
- StringLength: consente di specificare una lunghezza massima per una proprietà di stringa.
- Validation: classe di base per tutti gli attributi di validator.

> [!NOTE] 
> 
> Se le esigenze di convalida non sono soddisfatte da alcun validator standard, è sempre possibile creare un attributo validator personalizzato ereditando un nuovo attributo validator dall'attributo di convalida di base.

La classe Product nel **Listato 1** illustra come usare questi attributi validator. Il nome, la descrizione e le proprietà PrezzoUnitario sono contrassegnati come obbligatori. La lunghezza della proprietà Name deve essere una stringa di lunghezza inferiore a 10 caratteri. Infine, la proprietà PrezzoUnitario deve corrispondere a un criterio di espressione regolare che rappresenta un importo di valuta.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Listato 1**: Models\Product.vb

La classe Product illustra come usare un attributo aggiuntivo, ovvero l'attributo DisplayName. L'attributo DisplayName consente di modificare il nome della proprietà quando la proprietà viene visualizzata in un messaggio di errore. Anziché visualizzare il messaggio di errore "il campo PrezzoUnitario è obbligatorio", è possibile visualizzare il messaggio di errore "il campo Price è obbligatorio".

> [!NOTE] 
> 
> Se si desidera personalizzare completamente il messaggio di errore visualizzato da un validator, è possibile assegnare un messaggio di errore personalizzato alla proprietà ErrorMessage del validator come segue: `<Required(ErrorMessage:="This field needs a value!")>`

È possibile utilizzare la classe Product nel **Listato 1** con l'azione del controller Create () nel **Listato 2**. Questa azione del controller Visualizza nuovamente la vista crea quando lo stato del modello contiene errori.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Listato 2**: Controllers\ProductController.vb

Infine, è possibile creare la vista in **Listing 3** facendo clic con il pulsante destro del mouse sull'azione crea () e selezionando l'opzione di menu **Aggiungi visualizzazione**. Creare una visualizzazione fortemente tipizzata con la classe Product come classe del modello. Selezionare **Crea** nell'elenco a discesa Visualizza contenuto (vedere la **Figura 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figura 2**: aggiunta della visualizzazione di creazione

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Listato 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Rimuovere il campo ID dal modulo di creazione generato dall'opzione di menu **Aggiungi visualizzazione** . Poiché il campo ID corrisponde a una colonna Identity, non si vuole consentire agli utenti di immettere un valore per questo campo.

Se si invia il modulo per la creazione di un prodotto e non si immettono valori per i campi obbligatori, verranno visualizzati i messaggi di errore di convalida nella **Figura 3** .

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figura 3**: campi obbligatori mancanti

Se si immette un importo di valuta non valido, viene visualizzato il messaggio di errore nella **Figura 4** .

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figura 4**: importo di valuta non valido

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Uso di validator di annotazione dei dati con l'Entity Framework

Se si usa il Entity Framework Microsoft per generare le classi del modello di dati, non è possibile applicare gli attributi di convalida direttamente alle classi. Poiché il Entity Framework Designer genera le classi del modello, tutte le modifiche apportate alle classi del modello verranno sovrascritte alla successiva modifica della finestra di progettazione.

Se si desidera utilizzare i validator con le classi generate dal Entity Framework, è necessario creare classi di metadati. Si applicano i validator alla classe meta data anziché applicare i validator alla classe effettiva.

Si supponga, ad esempio, di aver creato una classe Movie usando il Entity Framework (vedere la **Figura 5**). Si supponga inoltre di voler fare in modo che le proprietà titolo film e Director siano obbligatorie. In tal caso, è possibile creare la classe parziale e la classe meta data nel **Listato 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figura 5**: classe Movie generata da Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Listato 4**: Models\Movie.vb

Il file nel **Listato 4** contiene due classi denominate Movie e MovieMetaData. La classe Movie è una classe parziale. Corrisponde alla classe parziale generata dal Entity Framework contenuto nel file DataModel. designer. vb.

Attualmente .NET Framework non supporta le proprietà parziali. Non è pertanto possibile applicare gli attributi validator alle proprietà della classe Movie definita nel file DataModel. designer. vb applicando gli attributi validator alle proprietà della classe Movie definita nel file nel **Listato 4**.

Si noti che la classe parziale Movie è decorata con un attributo MetadataType che punta alla classe MovieMetaData. La classe MovieMetaData contiene le proprietà del proxy per le proprietà della classe Movie.

Gli attributi di convalida vengono applicati alle proprietà della classe MovieMetaData. Le proprietà title, Director e DateReleased sono tutte contrassegnate come proprietà obbligatorie. Alla proprietà Director deve essere assegnata una stringa contenente meno di 5 caratteri. Infine, l'attributo DisplayName viene applicato alla proprietà DateReleased per visualizzare un messaggio di errore simile a "il campo Data di rilascio è obbligatorio". anziché l'errore "il campo DateReleased è obbligatorio".

> [!NOTE] 
> 
> Si noti che le proprietà del proxy nella classe MovieMetaData non devono rappresentare gli stessi tipi delle proprietà corrispondenti nella classe Movie. Ad esempio, la proprietà Director è una proprietà di stringa nella classe Movie e una proprietà dell'oggetto nella classe MovieMetaData.

La pagina nella **Figura 6** illustra i messaggi di errore restituiti quando si immettono valori non validi per le proprietà del film.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figura 6**: uso dei validator con la Entity Framework ([fare clic per visualizzare l'immagine con dimensioni complete](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Riepilogo

In questa esercitazione si è appreso come sfruttare lo strumento di associazione di modelli di annotazione dei dati per eseguire la convalida in un'applicazione MVC ASP.NET. Si è appreso come usare i diversi tipi di attributi di convalida, ad esempio gli attributi obbligatori e StringLength. Si è inoltre appreso come usare questi attributi quando si lavora con Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Precedente](validating-with-a-service-layer-vb.md)
