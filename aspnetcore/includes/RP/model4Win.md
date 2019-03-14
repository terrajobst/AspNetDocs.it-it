---
ms.openlocfilehash: 85fa9f8b18dc7dfb3e9d37703549dc5c28dc7a4a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062598"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Eseguire lo scaffolding del modello di filmato

* Eseguire il codice seguente dalla riga di comando nella directory del progetto che contiene i file *Program.cs*, *Startup.cs* e *csproj*:

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Se viene visualizzato l'errore:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

L'errore precedente si verifica quando ci si trova in una directory errata. Aprire una shell dei comandi nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*) e quindi eseguire il comando precedente.

Se viene visualizzato l'errore:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Chiudere Visual Studio ed eseguire nuovamente il comando.
