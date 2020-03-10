---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Convalida con un livello di servizio (VB) | Microsoft Docs
author: StephenWalther
description: Informazioni su come spostare la logica di convalida dalle azioni del controller e a un livello di servizio separato. In questa esercitazione, Stephen Walther spiega come...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 704657ffe6f50eaf3eb0d91d0d334567003ab7f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542826"
---
# <a name="validating-with-a-service-layer-vb"></a>Convalida con un livello di servizio (VB)

di [Stephen Walther](https://github.com/StephenWalther)

> Informazioni su come spostare la logica di convalida dalle azioni del controller e a un livello di servizio separato. In questa esercitazione, Stephen Walther spiega come è possibile mantenere una netta separazione delle problematiche isolando il livello di servizio dal livello del controller.

L'obiettivo di questa esercitazione è descrivere un metodo per eseguire la convalida in un'applicazione MVC ASP.NET. In questa esercitazione si apprenderà come spostare la logica di convalida dai controller e a un livello di servizio separato.

## <a name="separating-concerns"></a>Separazione delle problematiche

Quando si compila un'applicazione MVC ASP.NET, è consigliabile non inserire la logica del database all'interno delle azioni del controller. La combinazione della logica del database e del controller rende più difficile la gestione dell'applicazione nel corso del tempo. Si consiglia di inserire tutta la logica del database in un livello di repository separato.

Il listato 1, ad esempio, contiene un repository semplice denominato ProductRepository. Il repository del prodotto contiene tutto il codice di accesso ai dati per l'applicazione. L'elenco include anche l'interfaccia IProductRepository implementata dal repository del prodotto.

**Listato 1-Models\ProductRepository.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

Il controller nel listato 2 usa il livello di repository nelle azioni index () e create (). Si noti che questo controller non contiene alcuna logica di database. La creazione di un livello di repository consente di mantenere una netta separazione delle problematiche. I controller sono responsabili della logica di controllo del flusso dell'applicazione e il repository è responsabile della logica di accesso ai dati.

**Listato 2-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a>Creazione di un livello di servizio

Quindi, la logica di controllo del flusso dell'applicazione appartiene a un controller e la logica di accesso ai dati appartiene a un repository. In tal caso, dove si inserisce la logica di convalida? Un'opzione consiste nell'inserire la logica di convalida in un *livello di servizio*.

Un livello di servizio è un livello aggiuntivo in un'applicazione MVC ASP.NET che media la comunicazione tra un controller e un livello di repository. Il livello di servizio contiene la logica di business. In particolare, contiene la logica di convalida.

Ad esempio, il livello del servizio prodotto nel listato 3 ha un metodo CreateProduct (). Il metodo CreateProduct () chiama il metodo ValidateProduct () per convalidare un nuovo prodotto prima di passare il prodotto al repository del prodotto.

**Listato 3-Models\ProductService.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

Il controller di prodotto è stato aggiornato nel listato 4 per usare il livello di servizio anziché il livello del repository. Il livello controller comunica con il livello di servizio. Il livello di servizio comunica con il livello di repository. Ogni livello ha una responsabilità separata.

**Listato 4-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

Si noti che il servizio prodotto viene creato nel costruttore del controller del prodotto. Quando viene creato il servizio prodotto, il dizionario di stato del modello viene passato al servizio. Il servizio prodotto usa lo stato del modello per passare nuovamente i messaggi di errore di convalida al controller.

## <a name="decoupling-the-service-layer"></a>Separazione del livello di servizio

Non è stato possibile isolare i livelli del controller e del servizio con un solo aspetto. I livelli del controller e del servizio comunicano tramite lo stato del modello. In altre parole, il livello di servizio presenta una dipendenza da una particolare funzionalità del framework MVC ASP.NET.

Si vuole isolare il più possibile il livello di servizio dal livello di controller. In teoria, dovrebbe essere possibile usare il livello di servizio con qualsiasi tipo di applicazione e non solo un'applicazione MVC ASP.NET. In futuro, ad esempio, potrebbe essere necessario creare un front-end WPF per l'applicazione. Si dovrebbe trovare un modo per rimuovere la dipendenza dallo stato del modello MVC ASP.NET dal livello di servizio.

Nel listato 5, il livello di servizio è stato aggiornato in modo da non utilizzare più lo stato del modello. USA invece qualsiasi classe che implementa l'interfaccia IValidationDictionary.

**Listato 5-Models\ProductService.vb (disaccoppiato)**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

L'interfaccia IValidationDictionary è definita nel listato 6. Questa semplice interfaccia ha un solo metodo e una singola proprietà.

**Elenco 6-Models\IValidationDictionary.cs**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

La classe nel listato 7, denominata la classe ModelStateWrapper, implementa l'interfaccia IValidationDictionary. È possibile creare un'istanza della classe ModelStateWrapper passando un dizionario di stato del modello al costruttore.

**Listato 7-Models\ModelStateWrapper.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

Infine, il controller aggiornato nel listato 8 USA ModelStateWrapper durante la creazione del livello di servizio nel relativo costruttore.

**Listato 8-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

L'uso dell'interfaccia IValidationDictionary e della classe ModelStateWrapper ci consente di isolare completamente il livello di servizio dal livello del controller. Il livello di servizio non dipende più dallo stato del modello. È possibile passare qualsiasi classe che implementa l'interfaccia IValidationDictionary al livello di servizio. Ad esempio, un'applicazione WPF può implementare l'interfaccia IValidationDictionary con una classe Collection semplice.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è illustrare un approccio per eseguire la convalida in un'applicazione MVC ASP.NET. In questa esercitazione si è appreso come spostare tutta la logica di convalida dai controller e a un livello di servizio separato. Si è inoltre appreso come isolare il livello di servizio dal livello controller creando una classe ModelStateWrapper.

> [!div class="step-by-step"]
> [Precedente](validating-with-the-idataerrorinfo-interface-vb.md)
> [Successivo](validation-with-the-data-annotation-validators-vb.md)
