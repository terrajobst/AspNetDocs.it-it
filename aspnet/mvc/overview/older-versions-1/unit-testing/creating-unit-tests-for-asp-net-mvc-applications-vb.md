---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Creazione di unit test per applicazioni MVC ASP.NET (VB) | Microsoft Docs
author: StephenWalther
description: Informazioni su come creare unit test per le azioni del controller. In questa esercitazione, Stephen Walther illustra come verificare se un'azione del controller restituisce un parti...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 643faddaf6f9cd075131e8e5a9cccb303e355ceb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541433"
---
# <a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Creazione di unit test per le applicazioni ASP.NET MVC (VB)

di [Stephen Walther](https://github.com/StephenWalther)

[Scaricare PDF](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Informazioni su come creare unit test per le azioni del controller. In questa esercitazione, Stephen Walther illustra come verificare se un'azione del controller restituisce una determinata visualizzazione, restituisce un particolare set di dati o restituisce un tipo diverso di risultato dell'azione.

L'obiettivo di questa esercitazione è illustrare come scrivere unit test per i controller nelle applicazioni MVC ASP.NET. Viene illustrato come creare tre diversi tipi di unit test. Si apprenderà come testare la visualizzazione restituita da un'azione del controller, come testare i dati della visualizzazione restituiti da un'azione del controller e come verificare se un'azione del controller reindirizza l'utente a una seconda azione del controller.

## <a name="creating-the-controller-under-test"></a>Creazione del controller sottoposto a test

Si inizierà creando il controller che si desidera testare. Il controller, denominato `ProductController`, è contenuto nel listato 1.

**Listato 1-`ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

Il `ProductController` contiene due metodi di azione denominati `Index()` e `Details()`. Entrambi i metodi di azione restituiscono una visualizzazione. Si noti che l'azione `Details()` accetta un parametro denominato ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Test della visualizzazione restituita da un controller

Si supponga di voler verificare se il `ProductController` restituisce la visualizzazione a destra. È necessario assicurarsi che, quando viene richiamata l'azione `ProductController.Details()`, venga restituita la visualizzazione dettagli. La classe di test nel listato 2 contiene un unit test per il test della vista restituita dall'azione `ProductController.Details()`.

**Listato 2: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

La classe nel listato 2 include un metodo di test denominato `TestDetailsView()`. Questo metodo contiene tre righe di codice. La prima riga di codice crea una nuova istanza della classe `ProductController`. La seconda riga di codice richiama il metodo di azione `Details()` del controller. Infine, l'ultima riga di codice controlla se la vista restituita dal `Details()` azione è la visualizzazione dettagli.

La proprietà `ViewResult.ViewName` rappresenta il nome della vista restituita da un controller. Un grande avviso relativo al test di questa proprietà. Un controller può restituire una visualizzazione in due modi. Un controller può restituire in modo esplicito una visualizzazione simile alla seguente:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

In alternativa, il nome della vista può essere dedotto dal nome dell'azione del controller, come indicato di seguito:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Questa azione del controller restituisce anche una visualizzazione denominata `Details`. Tuttavia, il nome della vista viene dedotto dal nome dell'azione. Se si desidera testare il nome della vista, è necessario restituire in modo esplicito il nome della vista dall'azione del controller.

È possibile eseguire il unit test nel listato 2 immettendo la combinazione di tasti **CTRL + R, a** oppure facendo clic sul pulsante **Esegui tutti i test nella soluzione** (vedere la figura 1). Se il test viene superato, verrà visualizzata la finestra Risultati test nella figura 2.

[![eseguire tutti i test nella soluzione](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Figura 01**: eseguire tutti i test nella soluzione ([fare clic per visualizzare l'immagine con dimensioni complete](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))

[![esito positivo.](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Figura 02**: operazione riuscita. ([Fare clic per visualizzare l'immagine con dimensioni complete](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))

## <a name="testing-the-view-data-returned-by-a-controller"></a>Test dei dati di visualizzazione restituiti da un controller

Un controller MVC passa i dati a una visualizzazione usando un elemento denominato *`View Data`* . Si supponga, ad esempio, di voler visualizzare i dettagli di un determinato prodotto quando si richiama l'azione `ProductController Details()`. In tal caso, è possibile creare un'istanza di una classe `Product` (definita nel modello) e passare l'istanza alla visualizzazione `Details` sfruttando `View Data`.

Il `ProductController` modificato nel listato 3 include un'azione `Details()` aggiornata che restituisce un prodotto.

**Listato 3-`ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

In primo luogo, l'azione `Details()` crea una nuova istanza della classe `Product` che rappresenta un computer portatile. Successivamente, l'istanza della classe `Product` viene passata come secondo parametro al metodo `View()`.

È possibile scrivere unit test per verificare se i dati previsti sono contenuti nei dati della visualizzazione. Il unit test nel listato 4 verifica se viene restituito un prodotto che rappresenta un computer portatile quando si chiama il metodo di azione `ProductController Details()`.

**Listato 4-`ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

Nel listato 4, il metodo `TestDetailsView()` testa i dati di visualizzazione restituiti richiamando il metodo `Details()`. Il `ViewData` viene esposto come proprietà nell'`ViewResult` restituito richiamando il metodo `Details()`. La proprietà `ViewData.Model` contiene il prodotto passato alla visualizzazione. Il test verifica semplicemente che il nome del prodotto contenuto nei dati della vista sia il computer portatile.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Test del risultato dell'azione restituito da un controller

Un'azione del controller più complessa può restituire tipi diversi di risultati di azione a seconda dei valori dei parametri passati all'azione del controller. Un'azione del controller può restituire vari tipi di risultati dell'azione, tra cui un `ViewResult`, `RedirectToRouteResult`o `JsonResult`.

Ad esempio, l'azione `Details()` modificata nel listato 5 restituisce la visualizzazione `Details` quando si passa un ID prodotto valido all'azione. Se si passa un ID prodotto non valido, ovvero un ID con un valore minore di 1, si verrà reindirizzati all'azione `Index()`.

**Elenco 5-`ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

È possibile testare il comportamento dell'azione di `Details()` con il unit test nel listato 6. Il unit test nel listato 6 Verifica che venga reindirizzato alla vista `Index` quando viene passato un ID con valore-1 al metodo `Details()`.

**Elenco 6-`ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Quando si chiama il metodo `RedirectToAction()` in un'azione del controller, l'azione del controller restituisce una `RedirectToRouteResult`. Il test controlla se il `RedirectToRouteResult` reindirizza l'utente a un'azione del controller denominata `Index`.

## <a name="summary"></a>Riepilogo

In questa esercitazione si è appreso come compilare unit test per le azioni del controller MVC. In primo luogo, si è appreso come verificare se la visualizzazione corretta viene restituita da un'azione del controller. Si è appreso come usare la proprietà `ViewResult.ViewName` per verificare il nome di una vista.

Successivamente, è stato esaminato come è possibile testare il contenuto del `View Data`. Si è appreso come verificare se il prodotto corretto è stato restituito in `View Data` dopo la chiamata di un'azione del controller.

Infine, è stato illustrato come è possibile verificare se i diversi tipi di risultati dell'azione vengono restituiti da un'azione del controller. Si è appreso come verificare se un controller restituisce un `ViewResult` o una `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Precedente](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
