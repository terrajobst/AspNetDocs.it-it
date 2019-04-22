---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Creare oggetti di trasferimento dati (DTO) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 1af29955e8040c34840d4c77fc2006f59d2324dd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59395277"
---
# <a name="create-data-transfer-objects-dtos"></a>Creare oggetti di trasferimento dati (DTO)

da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://github.com/MikeWasson/BookService)

Diritto a questo punto, l'API web espone le entità del database al client. Il client riceve i dati che esegue il mapping direttamente alle tabelle di database. Tuttavia, che non è sempre una buona idea. Talvolta si desidera modificare la forma dei dati inviati al client. Può, ad esempio, essere necessario:

- Rimuovere i riferimenti circolari (vedere la sezione precedente).
- Nascondi proprietà specifiche che non dovrebbero per visualizzare i client.
- Omettere alcune proprietà per ridurre le dimensioni del payload.
- Convertire oggetti grafici contenenti oggetti annidati, in modo da renderli più pratico per i client.
- Evitare di "" eventuali vulnerabilità all'overposting. (Vedere [convalida dei modelli](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) per una discussione di evitare l'overposting.)
- Disaccoppiare il livello di servizio dal livello di database.

A tale scopo, è possibile definire un *oggetto di trasferimento dati* (DTO). Un oggetto DTO è un oggetto che definisce come i dati verranno inviati in rete. Vediamo come funziona con l'entità Book. Nella cartella Models, aggiungere due classi DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

Il `BookDetailDTO` classe include tutte le proprietà del modello di libro, con la differenza che `AuthorName` è una stringa che conterrà il nome dell'autore. Il `BookDTO` classe contiene un subset delle proprietà da `BookDetailDTO`.

Successivamente, sostituire i due metodi GET nel `BooksController` (classe), con le versioni che restituiscono oggetti DTO. Si userà il LINQ **seleziona** istruzione da convertire in oggetti DTO provenienti da entità Book.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Ecco il codice SQL generato dal nuovo `GetBooks` (metodo). È possibile vedere che Entity Framework traduce LINQ **seleziona** in un'istruzione SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Modificare infine la `PostBook` per restituire un oggetto DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> In questa esercitazione, abbiamo deciso di convertire agli oggetti DTO manualmente nel codice. Un'altra opzione consiste nell'usare una libreria, ad esempio [AutoMapper](http://automapper.org/) che gestisce automaticamente la conversione.
> 
> [!div class="step-by-step"]
> [Precedente](part-4.md)
> [Successivo](part-6.md)
