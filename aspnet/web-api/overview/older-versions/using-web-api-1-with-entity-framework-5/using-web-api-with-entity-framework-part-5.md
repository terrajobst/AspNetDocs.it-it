---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: "Parte 5: creazione di un'interfaccia utente dinamica con Knockout. js | Microsoft Docs"
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555993"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Parte 5: creazione di un'interfaccia utente dinamica con Knockout. js

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Creazione di un'interfaccia utente dinamica con Knockout.js

In questa sezione si userà knockout. js per aggiungere funzionalità alla visualizzazione amministratore.

[Knockout. js](http://knockoutjs.com/) è una libreria JavaScript che consente di associare facilmente i controlli HTML ai dati. Knockout. js usa il modello MVC (Model-View-ViewModel).

- Il *modello* è la rappresentazione lato server dei dati nel dominio aziendale (in questo caso, prodotti e ordini).
- La *visualizzazione* è il livello di presentazione (HTML).
- Il *modello di visualizzazione* è un oggetto JavaScript che include i dati del modello. Il modello di visualizzazione è un'astrazione del codice dell'interfaccia utente. Non conosce la rappresentazione HTML. Rappresenta invece le funzionalità astratte della vista, ad esempio "un elenco di elementi".

La vista è associata a dati al modello di visualizzazione. Gli aggiornamenti al modello di visualizzazione vengono riflessi automaticamente nella visualizzazione. Il modello di visualizzazione ottiene anche gli eventi dalla visualizzazione, ad esempio i clic sui pulsanti, ed esegue operazioni sul modello, ad esempio la creazione di un ordine.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Prima verrà definito il modello di visualizzazione. Successivamente, il markup HTML viene associato al modello di visualizzazione.

Aggiungere la sezione Razor seguente a admin. cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

È possibile aggiungere questa sezione in qualsiasi punto del file. Quando viene eseguito il rendering della visualizzazione, la sezione viene visualizzata nella parte inferiore della pagina HTML, immediatamente prima del tag di chiusura &lt;/Body&gt;.

Tutti gli script per questa pagina verranno inseriti nel tag script indicato dal commento:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Definire innanzitutto una classe del modello di visualizzazione:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**Ko. observableArray** è un tipo speciale di oggetto in Knockout, denominato *osservabile*. Dalla [documentazione di Knockout. js](http://knockoutjs.com/documentation/observables.html): un osservabile è un "oggetto JavaScript che può notificare ai Sottoscrittori le modifiche". Quando il contenuto di un oggetto osservabile cambia, la visualizzazione viene aggiornata automaticamente in modo che corrisponda a.

Per popolare la matrice di `products`, effettuare una richiesta AJAX all'API Web. Si tenga presente che l'URI di base per l'API è stato archiviato nel contenitore delle visualizzazioni (vedere la [parte 4](using-web-api-with-entity-framework-part-4.md) dell'esercitazione).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Aggiungere quindi funzioni al modello di visualizzazione per creare, aggiornare ed eliminare i prodotti. Queste funzioni inviano chiamate AJAX all'API Web e utilizzano i risultati per aggiornare il modello di visualizzazione.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

A questo punto, la parte più importante: quando il DOM è riempito, chiamare la funzione **Ko. applyBindings** e passare una nuova istanza della `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

Il metodo **Ko. applyBindings** attiva Knockout e collega il modello di visualizzazione alla visualizzazione.

Ora che è disponibile un modello di visualizzazione, è possibile creare le associazioni. In Knockout. js questa operazione viene eseguita aggiungendo `data-bind` attributi agli elementi HTML. Ad esempio, per associare un elenco HTML a una matrice, usare l'associazione `foreach`:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

L'associazione `foreach` scorre la matrice e crea elementi figlio per ogni oggetto nella matrice. Le associazioni negli elementi figlio possono fare riferimento alle proprietà degli oggetti matrice.

Aggiungere le associazioni seguenti all'elenco "Update-Products":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

L'elemento `<li>` si verifica nell'ambito dell'associazione **foreach** . Questo significa che il rendering dell'elemento viene eseguito una volta per ogni prodotto nell'array `products`. Tutte le associazioni all'interno dell'elemento `<li>` fanno riferimento a tale istanza del prodotto. `$data.Name`, ad esempio, fa riferimento alla proprietà `Name` sul prodotto.

Per impostare i valori degli input di testo, usare l'associazione `value`. I pulsanti sono associati alle funzioni nella visualizzazione modello, usando il binding `click`. L'istanza del prodotto viene passata come parametro a ogni funzione. Per ulteriori informazioni, la [documentazione di Knockout. js](http://knockoutjs.com/documentation/observables.html) include una descrizione corretta delle varie associazioni.

Successivamente, aggiungere un binding per l'evento **Submit** nel modulo Add Product:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Questa associazione chiama la funzione `create` sul modello di visualizzazione per creare un nuovo prodotto.

Ecco il codice completo per la visualizzazione amministratore:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Eseguire l'applicazione, accedere con l'account amministratore e fare clic sul collegamento "admin". Verrà visualizzato l'elenco dei prodotti e sarà possibile creare, aggiornare o eliminare i prodotti.

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-4.md)
> [Successivo](using-web-api-with-entity-framework-part-6.md)
