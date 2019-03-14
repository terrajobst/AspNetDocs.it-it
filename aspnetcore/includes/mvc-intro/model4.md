---
ms.openlocfilehash: 568fa161b27e554fd8b474670b8f9a7ec2f39f45
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047128"
---
Nella tabella seguente sono specificati i parametri del generatore di codice ASP.NET Core:

| Parametro               | Descrizione|
| ----------------- | ------------ |
| -m  | Nome del modello. |
| -dc  | Contesto dati. |
| -udl | Uso del layout predefinito. |
| --relativeFolderPath | Percorso relativo della cartella di output per creare le viste. |
| --useDefaultLayout | Per le viste deve essere usato il layout predefinito. |
| --referenceScriptLibraries | Aggiunge `_ValidationScriptsPartial` per modificare e creare pagine |

Utilizzare il commutatore `h` per ottenere assistenza sul comando `aspnet-codegenerator controller`:

```console
dotnet aspnet-codegenerator controller -h
```