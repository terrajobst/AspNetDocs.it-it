---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Aggiungere modelli e controller | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 57dacda421968f341284d89c9a3ad80040c16e25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557540"
---
# <a name="add-models-and-controllers"></a>Aggiungere modelli e controller

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione vengono aggiunte le classi del modello che definiscono le entità di database. Verranno quindi aggiunti controller API Web che eseguono operazioni CRUD su tali entità.

## <a name="add-model-classes"></a>Aggiungi classi modello

In questa esercitazione verrà creato il database usando l'approccio "Code First" per Entity Framework (EF). Con Code First, è possibile C# scrivere classi che corrispondono a tabelle di database e EF crea il database. Per ulteriori informazioni, vedere [Entity Framework approcci di sviluppo](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).

Si inizia definendo gli oggetti di dominio come oggetti POCO (Plain-Old CLR Objects). Si creeranno le seguenti operazioni POCO:

- Autore
- Book

In Esplora soluzioni, fare clic con il pulsante destro del mouse sulla cartella Models. Selezionare **Aggiungi**e quindi selezionare **classe**. Assegnare alla classe il nome `Author`.

![](part-2/_static/image1.png)

Sostituire tutto il codice standard in Author.cs con il codice seguente.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Aggiungere un'altra classe denominata `Book`con il codice seguente.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework utilizzeranno questi modelli per creare tabelle di database. Per ogni modello, la proprietà `Id` diventerà la colonna chiave primaria della tabella di database.

Nella classe Book, il `AuthorId` definisce una chiave esterna nella tabella `Author`. Per semplicità, si presuppone che ogni libro abbia un singolo autore. La classe Book contiene anche una proprietà di navigazione per la `Author`correlata. È possibile utilizzare la proprietà di navigazione per accedere al `Author` correlato nel codice. Si aggiungono altre informazioni sulle proprietà di navigazione nella parte 4, [gestione delle relazioni tra entità](part-4.md).

## <a name="add-web-api-controllers"></a>Aggiungere controller API Web

In questa sezione verranno aggiunti controller API Web che supportano operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione). I controller utilizzeranno Entity Framework per comunicare con il livello di database.

In primo luogo, è possibile eliminare i file Controllers/ValuesController. cs. Questo file contiene un esempio di controller API Web, ma non è necessario per questa esercitazione.

![](part-2/_static/image2.png)

Successivamente, compilare il progetto. L'impalcatura dell'API Web usa la reflection per trovare le classi del modello, quindi richiede l'assembly compilato.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Controllers. Selezionare **Aggiungi**, quindi **controller**.

![](part-2/_static/image3.png)

Nella finestra di dialogo **Aggiungi impalcatura** selezionare "Web API 2 controller with actions, using Entity Framework". Fare clic su **Aggiungi**.

![](part-2/_static/image4.png)

Nella finestra di dialogo **Aggiungi controller** eseguire le operazioni seguenti:

1. Nell'elenco a discesa **classe modello** selezionare la classe `Author`. Se non viene visualizzato nell'elenco a discesa, assicurarsi di aver compilato il progetto.
2. Selezionare "use Async controller actions".
3. Lasciare il nome del controller come &quot;AuthorsController&quot;.
4. Fare clic sul pulsante con il segno più (+) accanto alla **classe del contesto dati**.

![](part-2/_static/image5.png)

Nella finestra di dialogo **nuovo contesto dati** lasciare il nome predefinito e fare clic su **Aggiungi**.

![](part-2/_static/image6.png)

Fare clic su **Aggiungi** per completare la finestra di dialogo **Aggiungi controller** . La finestra di dialogo aggiunge due classi al progetto:

- `AuthorsController` definisce un controller API Web. Il controller implementa l'API REST usata dai client per eseguire operazioni CRUD nell'elenco di autori.
- `BookServiceContext` gestisce gli oggetti entità in fase di esecuzione, che include il popolamento di oggetti con dati da un database, il rilevamento delle modifiche e il salvataggio permanente dei dati nel database. Eredita da `DbContext`.

![](part-2/_static/image7.png)

A questo punto, compilare nuovamente il progetto. Eseguire ora la stessa procedura per aggiungere un controller API per le entità `Book`. Questa volta selezionare `Book` per la classe modello e selezionare la classe `BookServiceContext` esistente per la classe del contesto dati. Non creare un nuovo contesto dati. Fare clic su **Aggiungi** per aggiungere il controller.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Precedente](part-1.md)
> [Successivo](part-3.md)
