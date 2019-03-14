---
ms.openlocfilehash: 4d6b6309e6e15c364542b5d3ae296d53c6ea4358
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037398"
---
È necessario il codice di database di identità generato [migrazioni di Entity Framework Core](/ef/core/managing-schemas/migrations/). Creare una migrazione e aggiornare il database. Ad esempio, eseguire i comandi seguenti:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

In Visual Studio **Console di gestione pacchetti**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

Il parametro di nome "CreateIdentitySchema" per il `Add-Migration` comando è arbitrario. `"CreateIdentitySchema"` descrive la migrazione.
