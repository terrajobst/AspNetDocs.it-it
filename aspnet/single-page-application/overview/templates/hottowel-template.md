---
uid: single-page-application/overview/templates/hottowel-template
title: Modello di asciugamano caldo | Microsoft Docs
author: madskristensen
description: Modello HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075060"
---
# <a name="hot-towel-template"></a>Modello Hot Towel

di [Mads Kristensen](https://github.com/madskristensen)

> Il modello MVC Hot asciugamano è scritto da John Papa
> 
> Scegliere la versione da scaricare:
> 
> [Modello MVC Hot asciugamano per Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Modello MVC Hot asciugamano per Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Asciugamano caldo: perché non si vuole passare alla SPA senza una sola!

Si vuole creare un'applicazione a singola pagina, ma non è possibile decidere dove iniziare? Usa l'asciugamano caldo e in pochi secondi avrai a disposizione una SPA e tutti gli strumenti che ti servono per compilarli!

Hot asciugaman crea un ottimo punto di partenza per la creazione di un'applicazione a pagina singola (SPA) con ASP.NET. Che fornisce una struttura modulare per il codice, Visualizza la navigazione, data binding, la gestione avanzata dei dati e lo stile semplice ed elegante. L'asciugamano caldo offre tutto ciò che ti serve per creare un'applicazione a singola pagina, per consentirti di concentrarti sull'app, non sul plumbing.

> Scopri di più sulla creazione di un'applicazione SPA da [video, esercitazioni e corsi Pluralsight di John Papa](http://johnpapa.net/spa?vsix).

## <a name="application-structure"></a>Struttura dell'applicazione

Hot asciugamani SPA fornisce una cartella dell'app che contiene i file HTML e JavaScript che definiscono l'applicazione.

All'interno della cartella dell'app:

- Durandal
- servizi
- ViewModels
- viste

La cartella dell'app contiene una raccolta di moduli. Questi moduli incapsulano la funzionalità e dichiarano le dipendenze da altri moduli. La cartella Views contiene il codice HTML per l'applicazione e la cartella ViewModels contiene la logica di presentazione per le visualizzazioni (un modello MVVM comune). La cartella Services è la soluzione ideale per ospitare i servizi comuni che potrebbero essere necessari per l'applicazione, ad esempio il recupero dei dati HTTP o l'interazione di archiviazione locale. È comune che più ViewModel riutilizzino il codice dei moduli del servizio.

## <a name="aspnet-mvc-server-side-application-structure"></a>Struttura dell'applicazione lato server MVC ASP.NET

Hot asciugaman si basa sulla struttura di MVC ASP.NET familiare e potente.

- Avvio\_app
- Contenuto
- Controller
- Modelli
- Script
- Visualizzazioni

## <a name="featured-libraries"></a>Librerie in primo piano

- ASP.NET MVC
- ASP.NET Web API
- Ottimizzazione Web ASP.NET-aggregazione e minification
- Gestione avanzata dei dati di [Breeze. js](http://Breezejs.com)
- [Durandal. js](http://Durandaljs.com) -spostamento e composizione visualizzazione
- [Knockout. js](http://Knockoutjs.com) -associazioni dati
- [Require. js](http://requirejs.org) -modularità con AMD e ottimizzazione
- Popup [. js](http://jpapa.me/c7toastr) -messaggi popup
- [Bootstrap di Twitter](https://twitter.github.com/bootstrap/) -stile CSS affidabile

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Installazione tramite il modello Hot asciugamano SPA di Visual Studio 2012

Il tovagliolo caldo può essere installato come modello di Visual Studio 2012. È sufficiente fare clic su `File` | `New Project` e scegliere `ASP.NET MVC 4 Web Application`. Selezionare quindi il modello "applicazione a pagina singola Hot asciugamano" ed eseguire.

## <a name="installing-via-the-nuget-package"></a>Installazione tramite il pacchetto NuGet

Hot tovagliol è anche un pacchetto NuGet che aumenta un progetto MVC ASP.NET vuoto esistente. Basta installare usando NuGet, quindi eseguire.

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Come posso creare su un asciugamano caldo?

È sufficiente iniziare ad aggiungere codice.

1. Aggiungere il proprio codice sul lato server, preferibilmente Entity Framework e WebAPI (che davvero brilla con Breeze. js)
2. Aggiungere visualizzazioni alla cartella `App/views`
3. Aggiungere ViewModels alla cartella `App/viewmodels`
4. Aggiungere le associazioni dati HTML e Knockout alle nuove visualizzazioni
5. Aggiornare le route di navigazione in `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Procedura dettagliata di HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index. cshtml è la route iniziale e la visualizzazione per l'applicazione MVC. Contiene tutti i tag meta standard, i collegamenti CSS e i riferimenti JavaScript che ci si aspetterebbe. Il corpo contiene un solo `<div>`, dove tutto il contenuto (le visualizzazioni) verrà inserito quando richiesto. Il `@Scripts.Render` USA require. js per eseguire il punto di ingresso per il codice dell'applicazione, contenuto nel file di `main.js`. Viene fornita una schermata iniziale per dimostrare come creare una schermata iniziale durante il caricamento dell'app.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main. js

Il file di `main.js` contiene il codice che verrà eseguito non appena l'app viene caricata. Questo è il punto in cui si desidera definire le route di navigazione, impostare le visualizzazioni di avvio ed eseguire tutte le operazioni di installazione e bootstrap, ad esempio il priming dei dati dell'applicazione.

Il file di `main.js` definisce diversi moduli di Durandal che consentono di avviare l'applicazione. L'istruzione define consente di risolvere le dipendenze dei moduli in modo che siano disponibili per la funzione. Prima sono abilitati i messaggi di debug, che inviano messaggi sugli eventi che l'applicazione sta eseguendo alla finestra della console. Il codice dell'app. Start indica a Durandal Framework di avviare l'applicazione. Le convenzioni sono impostate in modo che Durandal sappia che tutte le visualizzazioni e gli ViewModel sono contenuti nelle stesse cartelle denominate, rispettivamente. Infine, il `app.setRoot` avvia il caricamento della `shell` mediante un'animazione `entrance` predefinita.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Visualizzazioni

Le visualizzazioni si trovano nella cartella `App/views`.

### <a name="shellhtml"></a>Shell. html

Il `shell.html` contiene il layout master per il codice HTML. Tutte le altre visualizzazioni verranno composte in un punto qualsiasi all'interno della vista `shell`. L'asciugamano caldo fornisce un `shell` con tre aree di questo tipo: un'intestazione, un'area di contenuto e un piè di pagina. Ognuna di queste aree viene caricata con il contenuto da altre visualizzazioni quando richiesto.

Le associazioni `compose` per l'intestazione e il piè di pagina sono hardcoded nell'asciugamano a caldo per puntare rispettivamente alle visualizzazioni `nav` e `footer`. Il binding compose per la sezione `#content` è associato all'elemento attivo del modulo di `router`. In altre parole, quando si fa clic su un collegamento di navigazione, viene caricata in quest'area la visualizzazione corrispondente.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV. html

Il `nav.html` contiene i collegamenti di navigazione per la SPA. Questo è il punto in cui è possibile inserire la struttura dei menu, ad esempio. Spesso si tratta di un binding dati (che usa Knockout) al modulo `router` per visualizzare la navigazione definita nell'`shell.js`. Knockout cerca gli attributi di associazione dei dati e li associa a `shell` ViewModel per visualizzare le route di navigazione e per visualizzare una ProgressBar (usando il bootstrap di Twitter) se il modulo di `router` sta passando da una visualizzazione all'altra (vedere `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home. html e Details. html

Queste visualizzazioni contengono codice HTML per le visualizzazioni personalizzate. Quando si fa clic sul collegamento `home` nel menu della vista `nav`, la visualizzazione `home` verrà inserita nell'area del contenuto della visualizzazione `shell`. Queste visualizzazioni possono essere ampliate o sostituite con visualizzazioni personalizzate.

### <a name="footerhtml"></a>footer. html

Il `footer.html` contiene il codice HTML visualizzato nel piè di pagina, nella parte inferiore della visualizzazione `shell`.

## <a name="viewmodels"></a>ViewModel

I ViewModel si trovano nella cartella `App/viewmodels`.

### <a name="shelljs"></a>Shell. js

Il ViewModel `shell` contiene le proprietà e le funzioni associate alla visualizzazione `shell`. Spesso è il punto in cui vengono trovate le associazioni di navigazione dei menu (vedere la logica di `router.mapNav`).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home. js e Details. js

Questi ViewModel contengono le proprietà e le funzioni associate alla visualizzazione `home`. contiene inoltre la logica di presentazione per la visualizzazione e rappresenta la collazione tra i dati e la visualizzazione.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Servizi

I servizi si trovano nella cartella app/Services. Idealmente, è possibile collocare i servizi futuri, ad esempio un modulo DataService, responsabile dell'acquisizione e della pubblicazione di dati remoti.

### <a name="loggerjs"></a>logger.js

L'asciugamano caldo fornisce un modulo `logger` nella cartella servizi. Il modulo `logger` è ideale per la registrazione dei messaggi nella console e per l'utente nei popup popup.
