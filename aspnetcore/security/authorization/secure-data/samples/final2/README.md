---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049108"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>Come compilare ed eseguire il campione di dati sicura degli utenti

* Impostazione della password con lo strumento Secret Manager:

  `dotnet user-secrets set SeedUserPW <pw>`

* Aggiornare il database:

    `dotnet ef database update`

* Abilitare HTTPS nel progetto
