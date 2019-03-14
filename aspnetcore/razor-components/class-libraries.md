---
title: Librerie di classi di componenti Razor
author: guardrex
description: Scopri come i componenti possono essere inclusi nelle App Razor componenti dalla libreria componente esterno.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 0e644627178bae2b8880760335860b3e0ebef156
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037408"
---
# <a name="razor-components-class-libraries"></a>Librerie di classi di componenti Razor

Da [Simon Timms](https://github.com/stimms)

> [!NOTE]
> il SDK di .NET Core 3.0 Preview 2 non include un modello di progetto per le librerie di classi Razor componente, ma si prevede di aggiungere un modello in una futura versione di anteprima. Nel frattempo, è possibile usare il modello libreria di classi componente Blazor illustrato in questo argomento.

I componenti possono essere condivise nelle librerie dei componenti tra progetti. I componenti possono essere inclusi da:

* Un altro progetto nella soluzione.
* Un pacchetto NuGet.
* Una libreria .NET di cui viene fatto riferimento.

Proprio come i componenti sono i tipi regolari di .NET, librerie dei componenti sono assembly .NET normale.

Per creare una nuova libreria di componenti, usare il `blazorlib` modello con il [dotnet nuovo](/dotnet/core/tools/dotnet-new) comando. Il modello fa parte dei modelli installati quando si [configurazione di componenti Razor](xref:razor-components/get-started).

```console
dotnet new blazorlib -o MyComponentLib1
```

Per aggiungere la libreria a un progetto esistente, usare il [dotnet sln](/dotnet/core/tools/dotnet-sln) comando:

```console
dotnet sln add .\MyComponentLib1
```

Librerie dei componenti possono contenere i file statici, ad esempio immagini, JavaScript e fogli di stile. In fase di compilazione, i file statici vengono incorporati nel file di assembly compilato (*DLL*), che consente l'utilizzo dei componenti senza la necessità di preoccuparsi di come includere le relative risorse. File inclusi nel `content` directory sono contrassegnate come risorsa incorporata. 

## <a name="consume-a-library-component"></a>Usare un componente della libreria

Per utilizzare i componenti definiti in una libreria in un altro progetto, il [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) direttiva deve essere utilizzata. È possibile aggiungere i singoli componenti in base al nome. Ad esempio, aggiunge la direttiva seguente `Component1` di `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Il formato generale della direttiva è:

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

Tuttavia, è comune per includere tutti i componenti da un assembly con un carattere jolly:

```cshtml
@addTagHelper *, MyComponentLib1
```

Il `@addTagHelper` direttiva può essere incluso nella *_ViewImport.cshtml* per rendere i componenti disponibili per un intero progetto o applicate a una singola pagina o un set di pagine all'interno di una cartella. Con la `@addTagHelper` direttiva posto, i componenti della libreria di componenti possono essere utilizzati come se fossero nello stesso assembly dell'app. 

## <a name="build-pack-and-ship-to-nuget"></a>Compilazione, Service pack e spedire a NuGet

Poiché le librerie dei componenti sono librerie .NET standard, creazione di pacchetti e l'invio in NuGet non è diverso da imballaggio e spedizione qualsiasi libreria da NuGet. Creazione di pacchetti viene eseguita utilizzando il [dotnet pack](/dotnet/core/tools/dotnet-pack) comando:

```console
dotnet pack
```

Caricare il pacchetto NuGet usando il [dotnet nuget pubblicare](/dotnet/core/tools/dotnet-nuget-push) comando:

```console
dotnet nuget publish
```

Tutte le risorse statiche incluse sono inclusi nel pacchetto NuGet. Consumer della libreria ricevono automaticamente gli script e fogli di stile, in modo che i consumer non è necessari installare manualmente le risorse.
