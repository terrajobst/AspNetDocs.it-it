---
uid: web-api/overview/advanced/dependency-injection
title: Inserimento di dipendenze in API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: Questa esercitazione illustra come inserire le dipendenze nel controller di API Web ASP.NET per ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: f9c212af92168ac02644625b9aa8ec1bef329cab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622605"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a>Inserimento di dipendenze in API Web ASP.NET 2

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Questa esercitazione illustra come inserire le dipendenze nel controller API Web ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - API Web 2
> - [Blocco di applicazioni Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (funziona anche la versione 5)

## <a name="what-is-dependency-injection"></a>Che cos'è l'inserimento delle dipendenze?

Una *dipendenza* è qualsiasi oggetto richiesto da un altro oggetto. Ad esempio, è comune definire un [repository](http://martinfowler.com/eaaCatalog/repository.html) che gestisce l'accesso ai dati. Di seguito viene illustrato un esempio. In primo luogo, verrà definito un modello di dominio:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Ecco una semplice classe di repository che archivia gli elementi in un database, usando Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

A questo punto è possibile definire un controller API Web che supporta le richieste GET per le entità `Product`. (Per semplicità, sto lasciando un POST e altri metodi). Ecco un primo tentativo:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Si noti che la classe controller dipende da `ProductRepository`e si consente al controller di creare l'istanza di `ProductRepository`. Tuttavia, non è una buona idea codificare la dipendenza in questo modo, per diversi motivi.

- Se si desidera sostituire `ProductRepository` con un'implementazione diversa, è necessario modificare anche la classe controller.
- Se il `ProductRepository` presenta dipendenze, è necessario configurarle all'interno del controller. Per un progetto di grandi dimensioni con più controller, il codice di configurazione viene sparso nel progetto.
- È difficile unit test perché il controller è hardcoded per eseguire query sul database. Per una unit test, è consigliabile usare un repository fittizio o stub, che non è possibile con la progettazione corrente.

Per risolvere questi problemi, *inserire* il repository nel controller. Per prima cosa, effettuare il refactoring della classe `ProductRepository` in un'interfaccia:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Fornire quindi il `IProductRepository` come parametro del costruttore:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

In questo esempio viene usato il [Costruttore Injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). È anche possibile usare l' *inserimento di Setter*, in cui impostare la dipendenza tramite un metodo di impostazione o una proprietà.

Ma ora si verifica un problema, perché l'applicazione non crea direttamente il controller. L'API Web crea il controller quando instrada la richiesta e l'API Web non conosce alcuna `IProductRepository`. Qui viene visualizzato il sistema di risoluzione delle dipendenze dell'API Web.

## <a name="the-web-api-dependency-resolver"></a>Sistema di risoluzione delle dipendenze dell'API Web

L'API Web definisce l'interfaccia **IDependencyResolver** per la risoluzione delle dipendenze. Di seguito è illustrata la definizione dell'interfaccia:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

L'interfaccia **IDependencyScope** dispone di due metodi:

- **GetService** crea un'istanza di un tipo.
- **GetServices** crea una raccolta di oggetti di un tipo specificato.

Il metodo **IDependencyResolver** eredita **IDependencyScope** e aggiunge il metodo **BeginScope** . Più avanti in questa esercitazione parlerò degli ambiti.

Quando l'API Web crea un'istanza del controller, chiama prima **IDependencyResolver. GetService**, passando il tipo di controller. È possibile utilizzare questo hook di estendibilità per creare il controller, risolvendo eventuali dipendenze. Se **GetService** restituisce null, l'API Web Cerca un costruttore senza parametri sulla classe controller.

## <a name="dependency-resolution-with-the-unity-container"></a>Risoluzione delle dipendenze con il contenitore Unity

Sebbene sia possibile scrivere un'implementazione **IDependencyResolver** completa da zero, l'interfaccia è effettivamente progettata per fungere da Bridge tra l'API Web e i contenitori IOC esistenti.

Un contenitore IoC è un componente software responsabile della gestione delle dipendenze. È possibile registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti. Il contenitore calcola automaticamente le relazioni di dipendenza. Molti contenitori IoC consentono inoltre di controllare elementi come la durata e l'ambito degli oggetti.

> [!NOTE]
> "IoC" sta per "Inversion of Control", che è un modello generale in cui un framework chiama il codice dell'applicazione. Un contenitore IoC crea gli oggetti per l'utente, che "inverte" il normale flusso di controllo.

Per questa esercitazione si userà [Unity](https://msdn.microsoft.com/library/ff647202.aspx) da Microsoft patterns &amp; Practices. Altre librerie comuni includono [Castle Windsor](http://www.castleproject.org/), [Spring.NET](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)e [StructureMap](http://structuremap.github.io/documentation/). Per installare Unity, è possibile usare Gestione pacchetti NuGet. Dal menu **strumenti** in Visual Studio selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**. Nella finestra console di gestione pacchetti digitare il comando seguente:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Ecco un'implementazione di **IDependencyResolver** che esegue il wrapping di un contenitore Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Se il metodo **GetService** non è in grado di risolvere un tipo, deve restituire **null**. Se il metodo **GetServices** non riesce a risolvere un tipo, deve restituire un oggetto Collection vuoto. Non generare eccezioni per i tipi sconosciuti.

## <a name="configuring-the-dependency-resolver"></a>Configurazione del sistema di risoluzione delle dipendenze

Impostare il resolver di dipendenza sulla proprietà **DependencyResolver** dell'oggetto **HttpConfiguration** globale.

Il codice seguente registra l'interfaccia `IProductRepository` con Unity e quindi crea una `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Ambito di dipendenza e durata del controller

I controller vengono creati per ogni richiesta. Per gestire la durata degli oggetti, **IDependencyResolver** usa il concetto di *ambito*.

Il resolver di dipendenza associato all'oggetto **HttpConfiguration** ha ambito globale. Quando tramite l'API Web viene creato un controller, viene chiamato **BeginScope**. Questo metodo restituisce un **IDependencyScope** che rappresenta un ambito figlio.

API Web chiama quindi **GetService** sull'ambito figlio per creare il controller. Quando la richiesta è completa, l'API Web chiama il **metodo Dispose** nell'ambito figlio. Usare il metodo **Dispose** per eliminare le dipendenze del controller.

La modalità di implementazione di **BeginScope** dipende dal contenitore IOC. Per Unity, l'ambito corrisponde a un contenitore figlio:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

La maggior parte dei contenitori IoC presenta equivalenti simili.
