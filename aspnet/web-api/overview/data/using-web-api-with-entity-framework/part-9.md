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
# <a name="add-a-new-item-to-the-database"></a><span data-ttu-id="e64e5-102">Aggiungere un nuovo elemento al database</span><span class="sxs-lookup"><span data-stu-id="e64e5-102">Add a New Item to the Database</span></span>

<span data-ttu-id="e64e5-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e64e5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e64e5-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="e64e5-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e64e5-105">In questa sezione verrà aggiunta la possibilità per gli utenti di creare un nuovo libro.</span><span class="sxs-lookup"><span data-stu-id="e64e5-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="e64e5-106">In app. js aggiungere il codice seguente al modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="e64e5-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="e64e5-107">In index. cshtml sostituire il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="e64e5-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="e64e5-108">con:</span><span class="sxs-lookup"><span data-stu-id="e64e5-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="e64e5-109">Questo markup crea un modulo per l'invio di un nuovo autore.</span><span class="sxs-lookup"><span data-stu-id="e64e5-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="e64e5-110">I valori per l'elenco a discesa autore sono associati a dati al `authors` osservabile nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e64e5-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="e64e5-111">Per gli altri input del modulo, i valori vengono associati a dati alla proprietà `newBook` del modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e64e5-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="e64e5-112">Il gestore di invio nel form è associato alla funzione `addBook`:</span><span class="sxs-lookup"><span data-stu-id="e64e5-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="e64e5-113">La funzione `addBook` legge i valori correnti degli input del form con associazione a dati per creare un oggetto JSON.</span><span class="sxs-lookup"><span data-stu-id="e64e5-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="e64e5-114">Quindi invia l'oggetto JSON a `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="e64e5-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e64e5-115">[Precedente](part-8.md)
> [Successivo](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="e64e5-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
