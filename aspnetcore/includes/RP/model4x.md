---
ms.openlocfilehash: d875717d480fe234ac5a328493fcec6a268e37a8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039518"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Eseguire lo scaffolding del modello di filmato

* Eseguire il codice seguente dalla riga di comando nella directory del progetto che contiene i file *Program.cs*, *Startup.cs* e *csproj*:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Se viene visualizzato l'errore:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Aprire una shell dei comandi nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).
