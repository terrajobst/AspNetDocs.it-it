---
ms.openlocfilehash: 4d6b6309e6e15c364542b5d3ae296d53c6ea4358
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037398"
---
<span data-ttu-id="e2645-101">È necessario il codice di database di identità generato [migrazioni di Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="e2645-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="e2645-102">Creare una migrazione e aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="e2645-102">Create a migration and update the database.</span></span> <span data-ttu-id="e2645-103">Ad esempio, eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e2645-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e2645-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2645-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e2645-105">In Visual Studio **Console di gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="e2645-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e2645-106">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2645-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="e2645-107">Il parametro di nome "CreateIdentitySchema" per il `Add-Migration` comando è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="e2645-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="e2645-108">`"CreateIdentitySchema"` descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="e2645-108">`"CreateIdentitySchema"` describes the migration.</span></span>
