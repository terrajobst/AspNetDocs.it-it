---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188718"
---
> <span data-ttu-id="6d808-101">Alcuni comandi non sono supportati se l'app Usa SQLite come suo archivio dati di identità.</span><span class="sxs-lookup"><span data-stu-id="6d808-101">Some commands aren't supported if the app uses SQLite as its Identity data store.</span></span> <span data-ttu-id="6d808-102">A causa delle limitazioni nel motore di database, `Alter` comandi generano l'eccezione seguente:</span><span class="sxs-lookup"><span data-stu-id="6d808-102">Due to limitations in the database engine, `Alter` commands throw the following exception:</span></span>
>
> <span data-ttu-id="6d808-103">"System.NotSupportedException: SQLite non supporta questa operazione di migrazione."</span><span class="sxs-lookup"><span data-stu-id="6d808-103">"System.NotSupportedException: SQLite does not support this migration operation."</span></span> 
>
> <span data-ttu-id="6d808-104">Come soluzione alternativa, eseguire le migrazioni Code First per modificare le tabelle nel database.</span><span class="sxs-lookup"><span data-stu-id="6d808-104">As a work around, run Code First migrations on the database to change the tables.</span></span>
