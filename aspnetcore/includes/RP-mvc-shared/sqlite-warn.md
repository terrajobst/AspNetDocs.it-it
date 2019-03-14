---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048218"
---

> [!NOTE]
> Molte operazioni di modifica dello schema non sono supportate dal provider EF Core SQLite. Ad esempio l'aggiunta di una colonna è supportata, ma la rimozione di una colonna non è supportata. Se viene creata una migrazione per rimuovere una colonna, il `ef migrations add` comando ha esito positivo ma il `ef database update` comando ha esito negativo. Alcune di queste limitazioni possono essere risolti, scrivendo manualmente il codice di migrazioni per eseguire una ricompilazione della tabella. Ricompilazione tabella comporta:

>* Ridenominazione della tabella esistente.
>* Creare una nuova tabella.
>* Copia dei dati della vecchia tabella nella nuova tabella.
>* Eliminazione della tabella precedente.

Per altre informazioni, vedere le seguenti risorse:
> * [Limitazioni del provider di database SQLite per EF Core](/ef/core/providers/sqlite/limitations)
> * [Personalizzare il codice di migrazione](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Seeding dei dati](/ef/core/modeling/data-seeding)