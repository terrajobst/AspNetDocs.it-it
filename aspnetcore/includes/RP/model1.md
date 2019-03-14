---
ms.openlocfilehash: 934db70fe49ba9a5330b25596dc694e0ac50c04e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037898"
---
Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa sezione si aggiungono alcune classi per la gestione di filmati in un database. Si usano queste classi con [Entity Framework Core](/ef/core) (EF Core) per usare un database. EF Core è un framework object-relational mapping (ORM) che semplifica il codice di accesso ai dati che è necessario scrivere.

Le classi di modello create sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core. Definiscono le proprietà dei dati archiviati nel database.

In questa esercitazione si scrivono innanzitutto le classi di modello e EF Core crea il database. Un approccio alternativo non trattato in questo articolo consiste nel [generare classi del modello da un database esistente](/ef/core/get-started/aspnetcore/existing-db).

[Visualizzare o scaricare il campione ](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie).
