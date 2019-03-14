---
title: Eseguire il debug di componenti Razor
author: guardrex
description: Informazioni su come eseguire il debug di App Blazor e componenti di Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/debug
ms.openlocfilehash: fb7ddcf3ae40ec28a372adf724a293b375be28a1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034118"
---
# <a name="debug-razor-components"></a>Eseguire il debug di componenti Razor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

I componenti di Razor presenta alcune *primissime* supporto per il debug delle applicazioni Blazor lato client in esecuzione su WebAssembly in Chrome. Anche se questo supporto iniziale per il debug è molto limitato e unpolished, Mostra l'infrastruttura di debug base assemblati.

Eseguire il debug di un'app di Blazor lato client in Chrome:

* Creare un'app Blazor in `Debug` configurazione (il valore predefinito per le app non pubblicate).
* Eseguire l'app Blazor in Chrome (versione 70 o versione successiva).
* Con lo stato attivo sull'app (non nel Pannello di strumenti per sviluppatori, che probabilmente è necessario chiudere un'esperienza di debug chiarezza), selezionare la seguente Blazor specifico tasto di scelta rapida:
  * `Shift+Alt+D` in Windows/Linux
  * `Shift+Cmd+D` in macOS

Eseguire Chrome con il debug remoto abilitato per il debug dell'app Blazor. Se il debug remoto è disabilitato, una pagina di errore viene generata in Chrome. La pagina di errore è contenute le istruzioni per l'esecuzione di Chrome con la porta di debug aperto in modo che il proxy di debug Blazor possa connettersi all'app. *Chiudere tutte le istanze di Chrome* e riavviare Chrome come richiesto.

![Pagina di errore debug Blazor](https://user-images.githubusercontent.com/1874516/43123091-01ec0796-8ed8-11e8-844c-23b4e6e9d069.png)

Chrome è in esecuzione con il debug remoto abilitato, il tasto di scelta rapida di debug consente di aprire una nuova scheda del debugger. Dopo qualche istante, la *origini* scheda Visualizza un elenco degli assembly .NET nell'app. Espandere ciascun assembly e individuare il *cs*/*cshtml* disponibile per il debug dei file di origine. Impostare punti di interruzione, passare alla scheda dell'app e i punti di interruzione vengono raggiunti. Dopo un punto di interruzione viene raggiunto, passo a passo (`F10`) o riprendere (`F8`) normalmente.

![Debug Blazor](https://user-images.githubusercontent.com/1874516/43123060-efb0b3b0-8ed7-11e8-9ea5-97aa34247a0b.png)

Blazor fornisce un proxy di debug che implementa il [Chrome DevTools protocollo](https://chromedevtools.github.io/devtools-protocol/) e aumenta con il protocollo. Informazioni specifiche di NET. Quando viene premuto debug tasti di scelta rapida, Chrome DevTools Blazor punti in corrispondenza del proxy. Il proxy si connette alla finestra del browser si sta tentando di eseguire il debug (di conseguenza la necessità di abilitare il debug remoto).

È lecito chiedersi perché non utilizzare semplicemente il mapping di origine del browser. Mapping di origine Consenti browser eseguire il mapping dei file compilati i file di origine. Tuttavia, Blazor non esegue il mapping C# direttamente in JS/WASM (almeno non ancora). Al contrario, Blazor esegue interpretazione IL all'interno del browser, pertanto mapping di origine non sono rilevanti.

Si noti che sono le funzionalità del debugger **molto limitato**. È possibile attualmente solo:

* Impostare e rimuovere i punti di interruzione.
* Passo a passo il codice o resume (`F8`).
* Nel *variabili locali* visualizzati, osservare i valori delle variabili locali di tipo `int`, `string`, e `bool`.
* Vedere lo stack di chiamate, tra cui catene di chiamate che vanno da JavaScript in .NET e da .NET a JavaScript.

Si *non è possibile*:

* Osservare i valori di eventuali variabili locali che non sono un' `int`, `string`, o `bool`.
* Osservare i valori di proprietà della classe o campi.
* Passare il mouse sulle variabili per visualizzarne i relativi valori
* Valutare le espressioni nella console.
* Passaggio tra le chiamate asincrone.
* Eseguire più altri comuni scenari di debug.

Sviluppo di ulteriori scenari di debug è un'attenzione costante del team di progettazione.

## <a name="troubleshooting-tip"></a>Suggerimento di risoluzione dei problemi

Se si verificano errori, il suggerimento seguente può essere utile:

Nel **debugger** scheda, aprire gli strumenti di sviluppo nel browser. Nella console, eseguire `localStorage.clear()` per rimuovere i punti di interruzione.
