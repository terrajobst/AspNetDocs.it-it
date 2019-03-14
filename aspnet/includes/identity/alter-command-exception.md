---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188718"
---
> Alcuni comandi non sono supportati se l'app Usa SQLite come suo archivio dati di identità. A causa delle limitazioni nel motore di database, `Alter` comandi generano l'eccezione seguente:
>
> "System.NotSupportedException: SQLite non supporta questa operazione di migrazione." 
>
> Come soluzione alternativa, eseguire le migrazioni Code First per modificare le tabelle nel database.
