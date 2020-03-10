---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Aggiunta del livello di logica di business a un progetto che utilizza l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634834"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Aggiunta del livello di logica di business a un progetto che utilizza l'associazione di modelli e Web Form

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource. Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.
> 
> Questa esercitazione illustra come usare l'associazione di modelli con un livello di logica di business. Si imposterà il membro OnCallingDataMethods per specificare che per chiamare i metodi dati verrà usato un oggetto diverso dalla pagina corrente.
> 
> Questa esercitazione si basa sul progetto creato nelle parti [precedenti](retrieving-data.md) della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.

## <a name="what-youll-build"></a>Elementi da compilare

L'associazione di modelli consente di inserire il codice di interazione dei dati nel file code-behind per una pagina Web o in una classe della logica di business separata. Nelle esercitazioni precedenti è stato illustrato come usare i file code-behind per il codice di interazione dei dati. Questo approccio funziona per i siti di piccole dimensioni, ma può comportare la ripetizione del codice e una maggiore difficoltà quando si gestisce un sito di grandi dimensioni. Può anche essere molto difficile testare a livello di codice il codice che risiede nei file code-behind perché non esiste un livello di astrazione.

Per centralizzare il codice di interazione dei dati, è possibile creare un livello di logica di business che contenga tutta la logica per interagire con i dati. Il livello della logica di business viene quindi chiamato dalle pagine Web. In questa esercitazione viene illustrato come spostare tutto il codice scritto nelle esercitazioni precedenti in un livello di logica di business e quindi utilizzare tale codice dalle pagine.

In questa esercitazione si apprenderà come:

1. Spostare il codice dai file code-behind a un livello della logica di business
2. Modificare i controlli associati a dati per chiamare i metodi nel livello della logica di business

## <a name="create-business-logic-layer"></a>Crea livello di logica di business

A questo punto, si creerà la classe che viene chiamata dalle pagine Web. I metodi di questa classe hanno un aspetto simile ai metodi usati nelle esercitazioni precedenti e includono gli attributi del provider di valori.

In primo luogo, aggiungere una nuova cartella denominata **BLL**.

![Aggiungi cartella](adding-business-logic-layer/_static/image1.png)

Nella cartella BLL creare una nuova classe denominata **SchoolBL.cs**. Conterrà tutte le operazioni sui dati che in origine risiedevano nei file code-behind. I metodi sono quasi identici a quelli del file code-behind, ma includeranno alcune modifiche.

La modifica più importante da notare è che non è più in esecuzione il codice dall'interno di un'istanza della classe **Page** . La classe Page contiene il metodo **TryUpdateModel** e la proprietà **ModelState** . Quando questo codice viene spostato in un livello di logica di business, non è più disponibile un'istanza della classe di pagina per chiamare questi membri. Per aggirare questo problema, è necessario aggiungere un parametro **ModelMethodContext** a qualsiasi metodo che accede a TryUpdateModel o ModelState. Usare questo parametro ModelMethodContext per chiamare TryUpdateModel o recuperare ModelState. Non è necessario modificare alcun elemento nella pagina Web per tenere conto di questo nuovo parametro.

Sostituire il codice in SchoolBL.cs con il codice seguente.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Rivedere le pagine esistenti per recuperare i dati dal livello della logica di business

Infine, si convertiranno le pagine students. aspx, AddStudent. aspx e courses. aspx dall'utilizzo di query nel file code-behind per l'utilizzo del livello della logica di business.

Nei file code-behind per Students, AddStudent e Courses, eliminare o impostare come commento i metodi di query seguenti:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Nel file code-behind non dovrebbe essere presente codice che riguarda le operazioni sui dati.

Il gestore dell'evento **OnCallingDataMethods** consente di specificare un oggetto da utilizzare per i metodi di dati. In students. aspx aggiungere un valore per il gestore eventi e modificare i nomi dei metodi dei dati con i nomi dei metodi nella classe della logica di business.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Nel file code-behind per Students. aspx definire il gestore eventi per l'evento CallingDataMethods. In questo gestore eventi specificare la classe della logica di business per le operazioni sui dati.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

In AddStudent. aspx apportare modifiche simili.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

In courses. aspx apportare modifiche simili.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Eseguire l'applicazione e notare che tutte le pagine funzionano come in precedenza. La logica di convalida funziona anche correttamente.

## <a name="conclusion"></a>Conclusione

In questa esercitazione è stata ristrutturata l'applicazione in modo da usare un livello di accesso ai dati e un livello di logica di business. È stato specificato che i controlli dati utilizzano un oggetto che non è la pagina corrente per le operazioni sui dati.

> [!div class="step-by-step"]
> [Precedente](using-query-string-values-to-retrieve-data.md)
