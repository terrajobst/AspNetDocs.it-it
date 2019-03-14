---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049108"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="2d44c-101">Come compilare ed eseguire il campione di dati sicura degli utenti</span><span class="sxs-lookup"><span data-stu-id="2d44c-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="2d44c-102">Impostazione della password con lo strumento Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="2d44c-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="2d44c-103">Aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="2d44c-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="2d44c-104">Abilitare HTTPS nel progetto</span><span class="sxs-lookup"><span data-stu-id="2d44c-104">Enable HTTPS in the project</span></span>
