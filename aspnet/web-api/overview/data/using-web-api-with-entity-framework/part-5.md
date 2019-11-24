---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Creare oggetti Trasferimento dati (dto) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: fc0463420207eba764014b8ec7123c5150e38247
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445758"
---
# <a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="c4cf2-102">Creare oggetti di trasferimento dati (DTO)</span><span class="sxs-lookup"><span data-stu-id="c4cf2-102">Create Data Transfer Objects (DTOs)</span></span>

<span data-ttu-id="c4cf2-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c4cf2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c4cf2-104">Scarica progetto completato</span><span class="sxs-lookup"><span data-stu-id="c4cf2-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c4cf2-105">A questo punto, l'API Web espone le entità di database al client.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="c4cf2-106">Il client riceve i dati di cui viene eseguito il mapping direttamente alle tabelle del database.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="c4cf2-107">Tuttavia, non è sempre una scelta ideale.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-107">However, that's not always a good idea.</span></span> <span data-ttu-id="c4cf2-108">In alcuni casi si desidera modificare la forma dei dati inviati al client.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="c4cf2-109">Potresti, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c4cf2-109">For example, you might want to:</span></span>

- <span data-ttu-id="c4cf2-110">Rimuovere i riferimenti circolari (vedere la sezione precedente).</span><span class="sxs-lookup"><span data-stu-id="c4cf2-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="c4cf2-111">Nascondere determinate proprietà che i client non devono visualizzare.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="c4cf2-112">Omettere alcune proprietà per ridurre le dimensioni del payload.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="c4cf2-113">Rendere flat gli oggetti grafici che contengono oggetti annidati per renderli più convenienti per i client.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="c4cf2-114">Evitare le vulnerabilità di "overposting".</span><span class="sxs-lookup"><span data-stu-id="c4cf2-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="c4cf2-115">Per una discussione sull'overposting, vedere la pagina relativa alla [convalida del modello](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="c4cf2-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="c4cf2-116">Separare il livello di servizio dal livello di database.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="c4cf2-117">A tale scopo, è possibile definire un *oggetto di trasferimento dati* (dto).</span><span class="sxs-lookup"><span data-stu-id="c4cf2-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="c4cf2-118">Un DTO è un oggetto che definisce il modo in cui i dati verranno inviati in rete.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="c4cf2-119">Vediamo come funziona con l'entità Book.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="c4cf2-120">Nella cartella Models (modelli) aggiungere due classi DTO:</span><span class="sxs-lookup"><span data-stu-id="c4cf2-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="c4cf2-121">La classe `BookDetailDto` include tutte le proprietà del modello Book, ad eccezione del fatto che `AuthorName` è una stringa che conterrà il nome dell'autore.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-121">The `BookDetailDto` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="c4cf2-122">La classe `BookDto` contiene un subset di proprietà di `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-122">The `BookDto` class contains a subset of properties from `BookDetailDto`.</span></span>

<span data-ttu-id="c4cf2-123">Sostituire quindi i due metodi GET nella classe `BooksController`, con le versioni che restituiscono dto.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="c4cf2-124">Si userà l'istruzione LINQ **Select** per eseguire la conversione da entità Book a dto.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="c4cf2-125">Ecco il SQL generato dal nuovo metodo `GetBooks`.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="c4cf2-126">Come si può notare, EF converte LINQ **Select** in un'istruzione SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="c4cf2-127">Infine, modificare il metodo `PostBook` per restituire un DTO.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="c4cf2-128">In questa esercitazione viene eseguita la conversione manuale in dto nel codice.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="c4cf2-129">Un'altra opzione consiste nell'usare una libreria come [automapper](http://automapper.org/) che gestisce automaticamente la conversione.</span><span class="sxs-lookup"><span data-stu-id="c4cf2-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="c4cf2-130">[Precedente](part-4.md)
> [Successivo](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="c4cf2-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
