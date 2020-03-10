---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Creare il client JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622346"
---
# <a name="create-the-javascript-client"></a>Creare il client JavaScript

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si creerà il client per l'applicazione, usando HTML, JavaScript e la libreria [knockout. js](http://knockoutjs.com/) . L'app client verrà compilata in fasi:

- Visualizzazione di un elenco di libri.
- Visualizzazione di un dettaglio del libro.
- Aggiunta di un nuovo libro.

La libreria Knockout usa il modello MVC (Model-View-ViewModel):

- Il **modello** è la rappresentazione lato server dei dati nel dominio aziendale (in questo caso, Books and Authors).
- La **visualizzazione** è il livello di presentazione (HTML).
- Il **modello di visualizzazione** è un oggetto JavaScript che include i modelli. Il modello di visualizzazione è un'astrazione del codice dell'interfaccia utente. Non conosce la rappresentazione HTML. Rappresenta invece le funzionalità astratte della vista, ad esempio &quot;un elenco di libri&quot;.

La vista è associata a dati al modello di visualizzazione. Gli aggiornamenti al modello di visualizzazione vengono riflessi automaticamente nella visualizzazione. Il modello di visualizzazione ottiene anche gli eventi dalla visualizzazione, ad esempio i clic sui pulsanti.

![](part-6/_static/image1.png)

Questo approccio semplifica la modifica del layout e dell'interfaccia utente dell'app, perché è possibile modificare i binding senza riscrivere il codice. Ad esempio, è possibile visualizzare un elenco di elementi come `<ul>`, quindi modificarlo in un secondo momento in una tabella.

## <a name="add-the-knockout-library"></a>Aggiungere la libreria Knockout

In Visual Studio scegliere **Gestione pacchetti NuGet**dal menu **strumenti** . Selezionare quindi **Console di Gestione pacchetti**. Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](part-6/samples/sample1.cmd)]

Questo comando aggiunge i file Knockout alla cartella Scripts.

## <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Aggiungere un file JavaScript denominato app. js alla cartella Scripts. (In Esplora soluzioni, fare clic con il pulsante destro del mouse sulla cartella script, scegliere **Aggiungi**, quindi selezionare **file JavaScript**). Incollare il codice seguente:

[!code-javascript[Main](part-6/samples/sample2.js)]

In Knockout la classe `observable` consente l'associazione dati. Quando il contenuto di una modifica osservabile, l'osservabile Invia notifiche a tutti i controlli con associazione a dati, in modo che possano aggiornarsi. La classe `observableArray` è la versione della matrice di *osservabile*. Per iniziare, il modello di visualizzazione presenta due osservabili:

- `books` include l'elenco dei libri.
- `error` contiene un messaggio di errore in caso di esito negativo di una chiamata AJAX.

Il metodo `getAllBooks` esegue una chiamata AJAX per ottenere l'elenco di libri. Quindi inserisce il risultato nella matrice `books`.

Il `ko.applyBindings` metodo fa parte della libreria knockout. Accetta il modello di visualizzazione come parametro e imposta il data binding.

## <a name="add-a-script-bundle"></a>Aggiungere un bundle di script

La creazione di bundle è una funzionalità di ASP.NET 4,5 che semplifica la combinazione o l'aggregazione di più file in un unico file. La creazione di bundle riduce il numero di richieste al server, che può migliorare il tempo di caricamento della pagina.

Aprire l'app file\_Start/BundleConfig. cs. Aggiungere il codice seguente al metodo RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Precedente](part-5.md)
> [Successivo](part-7.md)
