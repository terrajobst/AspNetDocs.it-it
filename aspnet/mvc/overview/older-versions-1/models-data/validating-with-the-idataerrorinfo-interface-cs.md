---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Convalida con l'interfaccia IDataErrorInfo (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther Mostra come visualizzare i messaggi di errore di convalida personalizzati implementando l'interfaccia IDataErrorInfo in una classe modello.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 938b180da02b1963acffd021d18621d75d1d0447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542560"
---
# <a name="validating-with-the-idataerrorinfo-interface-c"></a>Convalida con l'interfaccia IDataErrorInfo (C#)

di [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther Mostra come visualizzare i messaggi di errore di convalida personalizzati implementando l'interfaccia IDataErrorInfo in una classe modello.

L'obiettivo di questa esercitazione è illustrare un approccio per eseguire la convalida in un'applicazione MVC ASP.NET. Si apprenderà come impedire a un utente di inviare un modulo HTML senza fornire valori per i campi modulo richiesti. In questa esercitazione si apprenderà come eseguire la convalida usando l'interfaccia IErrorDataInfo.

## <a name="assumptions"></a>Presupposti

In questa esercitazione verranno usati il database MoviesDB e la tabella di database Movies. Questa tabella contiene le colonne seguenti:

<a id="0.5_table01"></a>

| **Nome colonna** | **Tipo di dati** | **Consenti valori NULL** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar(100) | False |
| Responsabile | Nvarchar(100) | False |
| DateReleased | DateTime | False |

In questa esercitazione viene utilizzato il Entity Framework Microsoft per generare le classi del modello di database. La classe film generata dal Entity Framework viene visualizzata nella figura 1.

[![l'entità Movie](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Figura 01**: entità Movie ([fare clic per visualizzare l'immagine con dimensioni complete](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))

> [!NOTE] 
> 
> Per ulteriori informazioni sull'utilizzo del Entity Framework per generare le classi del modello di database, vedere l'esercitazione relativa alla creazione di classi di modelli con l'Entity Framework.

## <a name="the-controller-class"></a>Classe controller

Il controller Home viene usato per elencare i film e creare nuovi filmati. Il codice per questa classe è contenuto nel listato 1.

**Listato 1-Controllers\HomeController.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

La classe del controller Home nel listato 1 contiene due azioni create (). La prima azione Visualizza il form HTML per la creazione di un nuovo film. La seconda azione Create () esegue l'inserimento effettivo del nuovo film nel database. La seconda azione Create () viene richiamata quando il form visualizzato dalla prima azione Create () viene inviato al server.

Si noti che la seconda azione Create () contiene le righe di codice seguenti:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

La proprietà IsValid restituisce false quando si verifica un errore di convalida. In tal caso, viene visualizzata nuovamente la vista crea che contiene il form HTML per la creazione di un film.

## <a name="creating-a-partial-class"></a>Creazione di una classe parziale

La classe Movie viene generata dal Entity Framework. È possibile visualizzare il codice per la classe Movie se si espande il file MoviesDBModel. edmx nella finestra Esplora soluzioni e si apre il file MoviesDBModel.Designer.cs nell'editor di codice (vedere la figura 2).

[![il codice per l'entità Movie](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Figura 02**: codice per l'entità Movie ([fare clic per visualizzare l'immagine con dimensioni complete](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))

La classe Movie è una classe parziale. Ciò significa che è possibile aggiungere un'altra classe parziale con lo stesso nome per estendere la funzionalità della classe Movie. La logica di convalida verrà aggiunta alla nuova classe parziale.

Aggiungere la classe nel listato 2 alla cartella Models.

**Listato 2-Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Si noti che la classe nel listato 2 include il modificatore *parziale* . Qualsiasi metodo o proprietà che si aggiunge a questa classe diventa parte della classe film generata dal Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Aggiunta di metodi parziali onmutator e OnChanged

Quando il Entity Framework genera una classe di entità, il Entity Framework aggiunge automaticamente metodi parziali alla classe. Il Entity Framework genera metodi parziali onmutanti e OnChanged che corrispondono a ogni proprietà della classe.

Nel caso della classe Movie, il Entity Framework crea i metodi seguenti:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Il metodo onmutator viene chiamato immediatamente prima della modifica della proprietà corrispondente. Il metodo OnChanged viene chiamato subito dopo la modifica della proprietà.

È possibile sfruttare questi metodi parziali per aggiungere la logica di convalida alla classe Movie. La classe Update Movie nel listato 3 Verifica che alle proprietà title e Director siano assegnati valori non vuoti.

> [!NOTE] 
> 
> Un metodo parziale è un metodo definito in una classe che non è necessario implementare. Se non si implementa un metodo parziale, il compilatore rimuove la firma del metodo e tutte le chiamate al metodo in modo che non vi siano costi in fase di esecuzione associati al metodo parziale. Nell'editor Visual Studio Code è possibile aggiungere un metodo parziale digitando la parola chiave *partial* seguito da uno spazio per visualizzare un elenco di parti parziali da implementare.

**Listato 3-Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Se ad esempio si tenta di assegnare una stringa vuota alla proprietà title, viene assegnato un messaggio di errore a un dizionario denominato \_Errors.

A questo punto, nulla si verifica effettivamente quando si assegna una stringa vuota alla proprietà title e viene aggiunto un errore al campo private \_Errors. È necessario implementare l'interfaccia IDataErrorInfo per esporre questi errori di convalida al framework ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementazione dell'interfaccia IDataErrorInfo

L'interfaccia IDataErrorInfo è stata inclusa in .NET Framework dalla prima versione. Questa interfaccia è un'interfaccia molto semplice:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Se una classe implementa l'interfaccia IDataErrorInfo, il framework ASP.NET MVC utilizzerà questa interfaccia durante la creazione di un'istanza della classe. Ad esempio, l'azione di creazione () del controller Home accetta un'istanza della classe Movie:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

Il Framework di MVC ASP.NET crea l'istanza del film passato all'azione Create () utilizzando uno strumento di associazione di modelli (DefaultModelBinder). Lo strumento di associazione di modelli è responsabile della creazione di un'istanza dell'oggetto Movie mediante l'associazione dei campi del modulo HTML a un'istanza dell'oggetto Movie.

DefaultModelBinder rileva se una classe implementa l'interfaccia IDataErrorInfo. Se una classe implementa questa interfaccia, lo strumento di associazione di modelli richiama IDataErrorInfo. questo indicizzatore per ogni proprietà della classe. Se l'indicizzatore restituisce un messaggio di errore, lo strumento di associazione di modelli aggiunge automaticamente questo messaggio di errore allo stato del modello.

DefaultModelBinder controlla anche la proprietà IDataErrorInfo. Error. Questa proprietà è destinata a rappresentare errori di convalida non specifici della proprietà associati alla classe. Ad esempio, potrebbe essere necessario applicare una regola di convalida che dipende dai valori di più proprietà della classe Movie. In tal caso, verrà restituito un errore di convalida dalla proprietà Error.

La classe Movie aggiornata nel listato 4 implementa l'interfaccia IDataErrorInfo.

**Listato 4-Models\Movie.cs (implementa IDataErrorInfo)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

Nel listato 4, la proprietà Indexer controlla la raccolta di errori \_per verificare se contiene una chiave corrispondente al nome della proprietà passata all'indicizzatore. Se non si verifica alcun errore di convalida associato alla proprietà, viene restituita una stringa vuota.

Non è necessario modificare il controller Home in alcun modo per usare la classe Movie modificata. La pagina visualizzata nella figura 3 illustra cosa accade quando non viene immesso alcun valore per i campi del modulo titolo o Director.

[![la creazione automatica di metodi di azione](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Figura 03**: un modulo con valori mancanti ([fare clic per visualizzare l'immagine con dimensioni complete](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))

Si noti che il valore DateReleased viene convalidato automaticamente. Poiché la proprietà DateReleased non accetta valori NULL, DefaultModelBinder genera automaticamente un errore di convalida per questa proprietà quando non dispone di un valore. Se si desidera modificare il messaggio di errore per la proprietà DateReleased, è necessario creare uno strumento di associazione di modelli personalizzato.

## <a name="summary"></a>Riepilogo

In questa esercitazione si è appreso come usare l'interfaccia IDataErrorInfo per generare messaggi di errore di convalida. In primo luogo, è stata creata una classe di film parziale che estende la funzionalità della classe di film parziale generata dal Entity Framework. Successivamente, è stata aggiunta la logica di convalida ai metodi parziali OnTitleChanging () e OnDirectorChanging () della classe Movie. Infine, è stata implementata l'interfaccia IDataErrorInfo per esporre questi messaggi di convalida al framework ASP.NET MVC.

> [!div class="step-by-step"]
> [Precedente](performing-simple-validation-cs.md)
> [Successivo](validating-with-a-service-layer-cs.md)
