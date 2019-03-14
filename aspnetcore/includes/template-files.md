---
ms.openlocfilehash: eb2f83504129ae2b19ff5d85a2bc7d90293b43d6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043398"
---
* Startup.cs: [Classe Startup](xref:fundamentals/startup) -classe consente di configurare la pipeline di richiesta che gestisce tutte le richieste effettuate all'applicazione.
* Program.cs: [Classe di programma](xref:fundamentals/index) che contiene il punto di ingresso principale dell'applicazione.
* firstapp.csproj: [File di progetto](/dotnet/articles/core/preview3/tools/csproj) formato di file di progetto MSBuild per le applicazioni ASP.NET Core. Contiene i riferimenti da progetto a progetto, i riferimenti NuGet e altri elementi correlati del progetto.
* appsettings.json / appsettings.Development.json : File di configurazione delle impostazioni di ambiente app di base. [Vedere la sezione configurazione](xref:fundamentals/configuration/index).
* bower.json : Dipendenze dei pacchetti bower per il progetto.
* . bowerrc: File di configurazione bower, che definisce la posizione in cui installare i componenti quando Bower Scarica le risorse.
* bundleconfig.JSON: file di configurazione per la creazione di bundle e minimizzazione di risorse front-end di JavaScript e CSS.
* Visualizzazioni: Contiene le visualizzazioni Razor. componenti che visualizzano l'interfaccia utente dell'app. In genere questa interfaccia utente presenta i dati del modello.
* Controller: Contiene i controller MVC, inizialmente *HomeController.cs*. I controller sono classi che gestiscono le richieste del browser.
* Wwwroot: Cartella radice dell'applicazione Web.

Per altre informazioni, vedere [lo schema MVC](xref:mvc/overview).
