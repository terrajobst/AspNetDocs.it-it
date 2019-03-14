---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048218"
---

> [!NOTE]
> <span data-ttu-id="4402f-101">Molte operazioni di modifica dello schema non sono supportate dal provider EF Core SQLite.</span><span class="sxs-lookup"><span data-stu-id="4402f-101">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="4402f-102">Ad esempio l'aggiunta di una colonna è supportata, ma la rimozione di una colonna non è supportata.</span><span class="sxs-lookup"><span data-stu-id="4402f-102">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="4402f-103">Se viene creata una migrazione per rimuovere una colonna, il `ef migrations add` comando ha esito positivo ma il `ef database update` comando ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="4402f-103">If a migration is created to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="4402f-104">Alcune di queste limitazioni possono essere risolti, scrivendo manualmente il codice di migrazioni per eseguire una ricompilazione della tabella.</span><span class="sxs-lookup"><span data-stu-id="4402f-104">Some of these limitations can be overcome by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="4402f-105">Ricompilazione tabella comporta:</span><span class="sxs-lookup"><span data-stu-id="4402f-105">A table rebuild involves:</span></span>

>* <span data-ttu-id="4402f-106">Ridenominazione della tabella esistente.</span><span class="sxs-lookup"><span data-stu-id="4402f-106">Renaming the existing table.</span></span>
>* <span data-ttu-id="4402f-107">Creare una nuova tabella.</span><span class="sxs-lookup"><span data-stu-id="4402f-107">Creating a new table.</span></span>
>* <span data-ttu-id="4402f-108">Copia dei dati della vecchia tabella nella nuova tabella.</span><span class="sxs-lookup"><span data-stu-id="4402f-108">Copying data from the old table to the new table.</span></span>
>* <span data-ttu-id="4402f-109">Eliminazione della tabella precedente.</span><span class="sxs-lookup"><span data-stu-id="4402f-109">Dropping the old table.</span></span>

<span data-ttu-id="4402f-110">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="4402f-110">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="4402f-111">Limitazioni del provider di database SQLite per EF Core</span><span class="sxs-lookup"><span data-stu-id="4402f-111">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="4402f-112">Personalizzare il codice di migrazione</span><span class="sxs-lookup"><span data-stu-id="4402f-112">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="4402f-113">Seeding dei dati</span><span class="sxs-lookup"><span data-stu-id="4402f-113">Data seeding</span></span>](/ef/core/modeling/data-seeding)