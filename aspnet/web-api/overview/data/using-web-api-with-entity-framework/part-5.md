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
# <a name="create-data-transfer-objects-dtos"></a>Creare oggetti di trasferimento dati (DTO)

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://github.com/MikeWasson/BookService)

A questo punto, l'API Web espone le entità di database al client. Il client riceve i dati di cui viene eseguito il mapping direttamente alle tabelle del database. Tuttavia, non è sempre una scelta ideale. In alcuni casi si desidera modificare la forma dei dati inviati al client. Può, ad esempio, essere necessario:

- Rimuovere i riferimenti circolari (vedere la sezione precedente).
- Nascondere determinate proprietà che i client non devono visualizzare.
- Omettere alcune proprietà per ridurre le dimensioni del payload.
- Rendere flat gli oggetti grafici che contengono oggetti annidati per renderli più convenienti per i client.
- Evitare le vulnerabilità di "overposting". Per una discussione sull'overposting, vedere la pagina relativa alla [convalida del modello](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) .
- Separare il livello di servizio dal livello di database.

A tale scopo, è possibile definire un *oggetto di trasferimento dati* (dto). Un DTO è un oggetto che definisce il modo in cui i dati verranno inviati in rete. Vediamo come funziona con l'entità Book. Nella cartella Models (modelli) aggiungere due classi DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

La classe `BookDetailDto` include tutte le proprietà del modello Book, ad eccezione del fatto che `AuthorName` è una stringa che conterrà il nome dell'autore. La classe `BookDto` contiene un subset di proprietà di `BookDetailDto`.

Sostituire quindi i due metodi GET nella classe `BooksController`, con le versioni che restituiscono dto. Si userà l'istruzione LINQ **Select** per eseguire la conversione da entità Book a dto.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Ecco il SQL generato dal nuovo metodo `GetBooks`. Come si può notare, EF converte LINQ **Select** in un'istruzione SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Infine, modificare il metodo `PostBook` per restituire un DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> In questa esercitazione viene eseguita la conversione manuale in dto nel codice. Un'altra opzione consiste nell'usare una libreria come [automapper](http://automapper.org/) che gestisce automaticamente la conversione.
> 
> [!div class="step-by-step"]
> [Precedente](part-4.md)
> [Successivo](part-6.md)
