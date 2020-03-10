---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Dettagli elemento visualizzato | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557323"
---
# <a name="display-item-details"></a>Visualizzare i dettagli degli elementi

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione verrà aggiunta la possibilità di visualizzare i dettagli di ogni libro. In app. js aggiungere il codice seguente al modello di visualizzazione:

[!code-javascript[Main](part-8/samples/sample1.js)]

In views/Home/index. cshtml aggiungere un elemento di associazione dati al collegamento Dettagli:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Questa operazione associa il gestore di clic per la &lt;un elemento&gt; alla funzione `getBookDetail` sul modello di visualizzazione.

Nello stesso file sostituire il contrassegno seguente:

[!code-html[Main](part-8/samples/sample3.html)]

con il seguente:

[!code-html[Main](part-8/samples/sample4.html)]

Questo markup crea una tabella con associazione a dati per le proprietà del `detail` osservabile nel modello di visualizzazione.

La sintassi "&lt;!--Ko--&gt;&quot; consente di includere un'associazione Knockout all'esterno di un elemento DOM. In questo caso, l'associazione `if` causa la visualizzazione di questa sezione di markup solo quando `details` è non null.

[!code-html[Main](part-8/samples/sample5.html)]

A questo punto, se si esegue l'app e si fa clic su uno dei &quot;dettagli&quot; collegamenti, l'app visualizzerà i dettagli del libro.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Precedente](part-7.md)
> [Successivo](part-9.md)
