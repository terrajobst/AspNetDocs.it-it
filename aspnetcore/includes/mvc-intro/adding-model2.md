---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044078"
---
## <a name="add-initial-migration-and-update-the-database"></a><span data-ttu-id="78554-101">Aggiungere la migrazione iniziale e l'aggiornamento del database</span><span class="sxs-lookup"><span data-stu-id="78554-101">Add initial migration and update the database</span></span>

* <span data-ttu-id="78554-102">Aprire un prompt dei comandi e passare alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="78554-102">Open a command prompt and navigate to the project directory.</span></span> <span data-ttu-id="78554-103">(La directory contenente il *Startup.cs* file).</span><span class="sxs-lookup"><span data-stu-id="78554-103">(The directory containing the *Startup.cs* file).</span></span>

* <span data-ttu-id="78554-104">Eseguire i comandi seguenti dal prompt dei comandi:</span><span class="sxs-lookup"><span data-stu-id="78554-104">Run the following commands in the command prompt:</span></span>

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  <span data-ttu-id="78554-105">[.NET core](/dotnet/core/tools/index) è un'implementazione multipiattaforma di .NET.</span><span class="sxs-lookup"><span data-stu-id="78554-105">[.NET Core](/dotnet/core/tools/index) is a cross-platform implementation of .NET.</span></span> <span data-ttu-id="78554-106">Ecco cosa eseguire questi comandi:</span><span class="sxs-lookup"><span data-stu-id="78554-106">Here is what these commands do:</span></span>

  * <span data-ttu-id="78554-107">[comando dotnet restore](/dotnet/core/tools/dotnet-restore): Scaricare i pacchetti NuGet specificati nel *file con estensione csproj* file.</span><span class="sxs-lookup"><span data-stu-id="78554-107">[dotnet restore](/dotnet/core/tools/dotnet-restore): Downloads the NuGet packages specified in the *.csproj* file.</span></span>
  * <span data-ttu-id="78554-108">`dotnet ef migrations add Initial` Esegue il comando di migrazioni di Entity Framework .NET Core CLI e crea la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="78554-108">`dotnet ef migrations add Initial` Runs the Entity Framework .NET Core CLI migrations command and creates the initial migration.</span></span> <span data-ttu-id="78554-109">Il parametro dopo "Aggiungi" è un nome assegnato per la migrazione.</span><span class="sxs-lookup"><span data-stu-id="78554-109">The parameter after "add" is a name that you assign to the migration.</span></span> <span data-ttu-id="78554-110">Qui si denominano la migrazione "Iniziale" perché è la migrazione del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="78554-110">Here you're naming the migration "Initial" because it's the initial database migration.</span></span> <span data-ttu-id="78554-111">Questa operazione crea il *dati/Migrations/\<data e ora > Initial* file che contiene i comandi di migrazione per aggiungere il *film* tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="78554-111">This operation creates the *Data/Migrations/\<date-time>_Initial.cs* file containing the migration commands to add the *Movie* table to the database.</span></span>
  * <span data-ttu-id="78554-112">`dotnet ef database update`  Aggiorna il database con la migrazione che è appena stato creato.</span><span class="sxs-lookup"><span data-stu-id="78554-112">`dotnet ef database update`  Updates the database with the migration we just created.</span></span>

<span data-ttu-id="78554-113">Nella prossima esercitazione si apprenderà sulla stringa di connessione e database.</span><span class="sxs-lookup"><span data-stu-id="78554-113">You'll learn about the database and connection string in the next tutorial.</span></span> <span data-ttu-id="78554-114">Verranno illustrate le modifiche al modello di dati nel [aggiungere un campo](xref:tutorials/first-mvc-app/new-field) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="78554-114">You'll learn about data model changes in the [Add a field](xref:tutorials/first-mvc-app/new-field) tutorial.</span></span>
