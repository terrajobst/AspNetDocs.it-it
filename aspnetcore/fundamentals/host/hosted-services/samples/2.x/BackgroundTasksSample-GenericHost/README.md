---
ms.openlocfilehash: 6b2bc386ec179e786de205af0ca6dbd610e000d9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57056488"
---
# <a name="aspnet-core-background-tasks-sample-generic-host"></a>Esempio di attività in background ASP.NET Core (host generico)

In questo esempio viene illustrato l'uso di [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice). L'esempio presenta le funzionalità descritte nell'argomento [Attività in background con servizi ospitati in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services).

Per l'esecuzione dell'esempio in [Visual Studio Code](https://code.visualstudio.com/), impostare il valore **console** della configurazione della console in *.vscode/launch.json* su `externalTerminal` o `integratedTerminal`. L'uso di `internalConsole` non è compatibile con l'input sequenza di tasti dalla console usato dall'app per accodare gli elementi di lavoro in background.
