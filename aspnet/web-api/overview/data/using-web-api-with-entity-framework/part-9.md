---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Aggiungere un nuovo elemento al database | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 692269a2c11e529af78f24feca74bba704b5b54b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557288"
---
# <a name="add-a-new-item-to-the-database"></a>Aggiungere un nuovo elemento al database

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione verrà aggiunta la possibilità per gli utenti di creare un nuovo libro. In app. js aggiungere il codice seguente al modello di visualizzazione:

[!code-javascript[Main](part-9/samples/sample1.js)]

In index. cshtml sostituire il markup seguente:

[!code-html[Main](part-9/samples/sample2.html)]

con:

[!code-html[Main](part-9/samples/sample3.html)]

Questo markup crea un modulo per l'invio di un nuovo autore. I valori per l'elenco a discesa autore sono associati a dati al `authors` osservabile nel modello di visualizzazione. Per gli altri input del modulo, i valori vengono associati a dati alla proprietà `newBook` del modello di visualizzazione.

Il gestore di invio nel form è associato alla funzione `addBook`:

[!code-html[Main](part-9/samples/sample4.html)]

La funzione `addBook` legge i valori correnti degli input del form con associazione a dati per creare un oggetto JSON. Quindi invia l'oggetto JSON a `/api/books`.

> [!div class="step-by-step"]
> [Precedente](part-8.md)
> [Successivo](part-10.md)
