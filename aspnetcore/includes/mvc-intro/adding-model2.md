---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044078"
---
## <a name="add-initial-migration-and-update-the-database"></a>Aggiungere la migrazione iniziale e l'aggiornamento del database

* Aprire un prompt dei comandi e passare alla directory del progetto. (La directory contenente il *Startup.cs* file).

* Eseguire i comandi seguenti dal prompt dei comandi:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](/dotnet/core/tools/index) è un'implementazione multipiattaforma di .NET. Ecco cosa eseguire questi comandi:

  * [comando dotnet restore](/dotnet/core/tools/dotnet-restore): Scaricare i pacchetti NuGet specificati nel *file con estensione csproj* file.
  * `dotnet ef migrations add Initial` Esegue il comando di migrazioni di Entity Framework .NET Core CLI e crea la migrazione iniziale. Il parametro dopo "Aggiungi" è un nome assegnato per la migrazione. Qui si denominano la migrazione "Iniziale" perché è la migrazione del database iniziale. Questa operazione crea il *dati/Migrations/\<data e ora > Initial* file che contiene i comandi di migrazione per aggiungere il *film* tabella nel database.
  * `dotnet ef database update`  Aggiorna il database con la migrazione che è appena stato creato.

Nella prossima esercitazione si apprenderà sulla stringa di connessione e database. Verranno illustrate le modifiche al modello di dati nel [aggiungere un campo](xref:tutorials/first-mvc-app/new-field) esercitazione.
