---
ms.openlocfilehash: 025a244812a50709bfd48de9f47a234a547c53e2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047418"
---
<!-- THIS INCLUDE USED BY MVC AND RP --> Aggiungere le proprietà seguenti alla classe `Movie`:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

La classe `Movie` contiene:

* Il campo `ID` è richiesto dal database per la chiave primaria.
* `[DataType(DataType.Date)]`:  L'attributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) specifica il tipo di dati (Data). Con questo attributo:

  * l'utente non deve immettere le informazioni temporali nel campo della data.
  * Viene visualizzata solo la data, non le informazioni temporali.

L'attributo [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) viene analizzato in un'esercitazione successiva.