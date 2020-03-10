---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Creare la visualizzazione (interfaccia utente) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557302"
---
# <a name="create-the-view-ui"></a>Creare la visualizzazione (interfaccia utente)

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si inizierà a definire il codice HTML per l'app e si aggiungeranno data binding tra il codice HTML e il modello di visualizzazione.

Aprire il file views/Home/index. cshtml. Sostituire l'intero contenuto del file con il codice seguente.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

La maggior parte degli elementi `div` è disponibile per lo stile [bootstrap](http://getbootstrap.com/) . Gli elementi importanti sono quelli con attributi `data-bind`. Questo attributo collega il codice HTML al modello di visualizzazione.

Esempio:

[!code-html[Main](part-7/samples/sample2.html)]

In questo esempio, il &quot;`text`&quot; binding fa in modo che l'elemento `<p>` visualizzi il valore della proprietà `error` dal modello di visualizzazione. Ricordare che `error` è stata dichiarata come `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Ogni volta che un nuovo valore viene assegnato a `error`, Knockout aggiorna il testo nell'elemento `<p>`.

Il binding `foreach` indica a knockout di scorrere il contenuto della matrice di `books`. Per ogni elemento nella matrice, Knockout crea un nuovo elemento &lt;li&gt;. Le associazioni all'interno del contesto del `foreach` fanno riferimento alle proprietà dell'elemento della matrice. Esempio:

[!code-html[Main](part-7/samples/sample4.html)]

Qui l'associazione `text` legge la proprietà Author di ogni libro.

Se l'applicazione viene eseguita adesso, dovrebbe avere un aspetto simile al seguente:

![](part-7/_static/image1.png)

L'elenco di libri viene caricato in modo asincrono dopo il caricamento della pagina. In questo momento, i collegamenti&quot; &quot;dettagli non funzionano. Questa funzionalità verrà aggiunta nella sezione successiva.

> [!div class="step-by-step"]
> [Precedente](part-6.md)
> [Successivo](part-8.md)
