---
ms.openlocfilehash: 85fa9f8b18dc7dfb3e9d37703549dc5c28dc7a4a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062598"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="3384c-101">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="3384c-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="3384c-102">Eseguire il codice seguente dalla riga di comando nella directory del progetto che contiene i file *Program.cs*, *Startup.cs* e *csproj*:</span><span class="sxs-lookup"><span data-stu-id="3384c-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="3384c-103">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="3384c-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="3384c-104">L'errore precedente si verifica quando ci si trova in una directory errata.</span><span class="sxs-lookup"><span data-stu-id="3384c-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="3384c-105">Aprire una shell dei comandi nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*) e quindi eseguire il comando precedente.</span><span class="sxs-lookup"><span data-stu-id="3384c-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="3384c-106">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="3384c-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="3384c-107">Chiudere Visual Studio ed eseguire nuovamente il comando.</span><span class="sxs-lookup"><span data-stu-id="3384c-107">Exit Visual Studio and run the command again.</span></span>
