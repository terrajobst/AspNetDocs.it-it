---
ms.openlocfilehash: 22c540058e0b3b9ae612f1b2612cecc08627ea2b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063988"
---
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="f7217-101">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="f7217-101">Test the app</span></span>

* <span data-ttu-id="f7217-102">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="f7217-102">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="f7217-103">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f7217-103">Test the **Create** link.</span></span>

  ![Pagina Crea](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="f7217-105">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="f7217-105">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="f7217-106">Se viene visualizzato l'errore seguente, verificare di aver eseguito le migrazioni e di aver aggiornato il database:</span><span class="sxs-lookup"><span data-stu-id="f7217-106">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```
