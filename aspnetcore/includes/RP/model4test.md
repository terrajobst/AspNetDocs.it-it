---
ms.openlocfilehash: 22c540058e0b3b9ae612f1b2612cecc08627ea2b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063988"
---
<a name="test"></a>
### <a name="test-the-app"></a>Eseguire il test dell'app

* Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).
* Eseguire il test del collegamento **Crea**.

  ![Pagina Crea](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.

Se viene visualizzato l'errore seguente, verificare di aver eseguito le migrazioni e di aver aggiornato il database:

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```
