---
title: Layout di Razor componenti
author: guardrex
description: Informazioni su come creare componenti riutilizzabili di layout per le app Blazor e componenti di Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/layouts
ms.openlocfilehash: 23d8f441c0b3bbde7a73717f6257013831617ec0
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039038"
---
# <a name="razor-components-layouts"></a>Layout di Razor componenti

Da [Stropek Rainer](https://www.timecockpit.com)

Le App in genere contengono più di una pagina. Gli elementi di layout, ad esempio menu, i messaggi sul copyright e i loghi, devono essere presenti in tutte le pagine. Copia il codice di questi elementi di layout in tutte le pagine di un'app non è una soluzione efficiente. Tale funzionalità è difficile da gestire e probabilmente conduce al contenuto incoerente nel corso del tempo. *Layout* risolvere questo problema.

Tecnicamente, un layout è semplicemente un altro componente. Un layout è definito in un modello Razor o in C# del codice e può contenere l'associazione dati, inserimento di dipendenze e altre funzionalità comuni dei componenti. Due aspetti aggiuntivi attiva una *component* in un *layout*:

* Il componente del layout deve ereditare da `BlazorLayoutComponent`. `BlazorLayoutComponent` definisce un `Body` proprietà che contiene il contenuto da sottoporre a rendering all'interno del layout.
* Il componente layout utilizza il `Body` proprietà per specificare dove deve essere il contenuto del corpo viene eseguito il rendering tramite la sintassi Razor `@Body`. Durante il rendering, `@Body` viene sostituito dal contenuto del layout.

Esempio di codice seguente viene illustrato il modello di un componente del layout Razor. Si noti l'uso del `BlazorLayoutComponent` e `@Body`:

```csharp
@inherits BlazorLayoutComponent

<header>
    <h1>ERP Master 3000</h1>
</header>

<nav>
    <a href="master-data">Master Data Management</a>
    <a href="invoicing">Invoicing</a>
    <a href="accounting">Accounting</a>
</nav>

@Body

<footer>
    &copy; by @CopyrightMessage
</footer>

@functions {
    public string CopyrightMessage { get; set; }
    ...
}
```

## <a name="use-a-layout-in-a-component"></a>Usare un layout in un componente

Usare la direttiva Razor `@layout` per applicare un layout a un componente. Il compilatore converte questa direttiva in un `LayoutAttribute`, cui viene applicata alla classe del componente.

Esempio di codice seguente viene illustrato il concetto. Il contenuto di questo componente viene inserito il *MasterLayout* in corrispondenza della posizione di `@Body`:

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a>Selezione di layout centralizzato

Ogni cartella di un un'app può contenere un file di modello denominato *viewimports. cshtml*. Il compilatore include le direttive specificate nel file di importazioni di visualizzazione in tutti i modelli Razor nella stessa cartella e in modo ricorsivo in tutte le relative sottocartelle. Pertanto, un *viewimports. cshtml* file contenente `@layout MainLayout` assicura che tutti i componenti in un cartella, usare i *MainLayout* layout. Non è necessario aggiungere più volte `@layout` a tutte le  *\*cshtml* file.

Si noti che il modello predefinito Usa la *viewimports. cshtml* meccanismo per la selezione di layout. Un'app appena creata contiene il *viewimports. cshtml* del file nel *pagine* cartella.

## <a name="nested-layouts"></a>Layout annidati

Le app possono essere costituito da layout annidati. Un componente può fare riferimento a un layout che a sua volta fa riferimento a un altro layout. Ad esempio, il layout di annidamento è utilizzabile in base a una struttura a più livelli.

Esempi di codice seguenti mostrano come usare layout annidati. Il *CustomersComponent.cshtml* file è il componente da visualizzare. Si noti che il componente vi fa riferimento il layout `MasterDataLayout`.

*CustomersComponent.cshtml*:

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

Il *MasterDataLayout.cshtml* fornisce file di `MasterDataLayout`. Il layout fa riferimento a un altro layout, `MainLayout`, in cui verrà incorporato.

*MasterDataLayout.cshtml*:

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

Infine, `MainLayout` contiene gli elementi di layout di primo livello, ad esempio l'intestazione, piè di pagina e menu principale.

*MainLayout.cshtml*:

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
