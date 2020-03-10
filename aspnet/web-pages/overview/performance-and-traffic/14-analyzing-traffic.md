---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Rilevamento delle informazioni sui visitatori (Analytics) per un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Al termine dell'operazione, potrebbe essere opportuno analizzare il traffico del sito Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525186"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Rilevamento delle informazioni sui visitatori (Analytics) per un sito di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come usare un helper per aggiungere analisi di siti Web alle pagine di un sito Web di Pagine Web ASP.NET (Razor).
> 
> Contenuto dell'esercitazione:
> 
> - Come inviare informazioni sul traffico del sito Web a un provider di analisi.
> 
> Queste sono le funzionalità di programmazione di ASP.NET introdotte nell'articolo:
> 
> - Helper `Analytics`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - ASP.NET Web Helper Library (pacchetto NuGet)

Analytics è un termine generale per la tecnologia che misura il traffico nel sito Web, in modo da poter comprendere il modo in cui gli utenti usano il sito. Sono disponibili molti servizi di analisi, inclusi i servizi di Google, Yahoo, StatCounter e altri.

Il funzionamento di Analytics è l'iscrizione a un account con il provider di analisi, in cui si registra il sito di cui si vuole tenere traccia. Il provider invia un frammento di codice JavaScript che include un ID o un codice di rilevamento per l'account. Aggiungere il frammento JavaScript alle pagine Web del sito di cui si vuole tenere traccia. Il frammento di analisi viene in genere aggiunto a un piè di pagina o a una pagina di layout o a un altro markup HTML visualizzato in ogni pagina del sito. Quando gli utenti richiedono una pagina che contiene uno di questi frammenti di codice JavaScript, il frammento Invia le informazioni sulla pagina corrente al provider di analisi, che registra diversi dettagli sulla pagina.

Quando si vuole esaminare le statistiche del sito, accedere al sito Web del provider di analisi. È quindi possibile visualizzare tutti i tipi di report relativi al sito, ad esempio:

- Numero di visualizzazioni di pagina per le singole pagine. Questo indica (approssimativamente) il numero di persone che visitano il sito e le pagine del sito più diffuse.
- Tempo impiegato dalle persone per le pagine specifiche. In questo modo è possibile sapere se la home page sta mantenendo l'interesse degli utenti.
- I siti in cui si trovavano persone prima di visitare il sito. Ciò consente di comprendere se il traffico è proveniente da collegamenti, da ricerche e così via.
- Quando gli utenti visitano il sito e il tempo di permanenza.
- Da quali paesi si trovano i visitatori.
- Quali browser e sistemi operativi usano i visitatori.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Uso di un helper per aggiungere analisi a una pagina

Pagine Web ASP.NET include diversi helper di analisi (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`e `Analytics.GetStatCounterHtml`) che semplificano la gestione dei frammenti di codice JavaScript usati per l'analisi. Invece di capire come e dove inserire il codice JavaScript, è sufficiente aggiungere l'helper a una pagina. Le uniche informazioni che è necessario fornire sono il nome dell'account, l'ID o il codice di verifica. Per StatCounter è inoltre necessario specificare alcuni valori aggiuntivi.

In questa procedura verrà creata una pagina di layout che usa l'helper `GetGoogleHtml`. Se si dispone già di un account con uno degli altri provider di analisi, è possibile usare tale account e apportare modifiche minime, se necessario.

> [!NOTE]
> Quando si crea un account Analytics, si registra l'URL del sito di cui si vuole tenere traccia. Se si esegue il testing di tutti gli elementi nel computer locale, non verrà eseguito il rilevamento del traffico effettivo (il solo traffico è), quindi non sarà possibile registrare e visualizzare le statistiche del sito. Questa procedura mostra però come aggiungere un helper di Analytics a una pagina. Quando si pubblica il sito, il sito attivo invierà informazioni al provider di analisi.

1. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato aggiunto.
2. Creare un account con Google Analytics e registrare il nome dell'account.
3. Creare una pagina di layout denominata *Analytics. cshtml* e aggiungere il markup seguente:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > È necessario inserire la chiamata all'helper `Analytics` nel corpo della pagina Web (prima del tag `</body>`). In caso contrario, il browser non eseguirà lo script.

    Se si usa un provider di analisi diverso, usare invece uno degli helper seguenti:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Sostituire `myaccount` con il nome dell'account, l'ID o il codice di rilevamento creato nel passaggio 1.
5. Eseguire la pagina nel browser. Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla.
6. Nel browser visualizzare l'origine della pagina. Sarà possibile visualizzare il codice di analisi di cui è stato eseguito il rendering:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Accedere al sito di Google Analytics ed esaminare le statistiche per il sito. Se la pagina viene eseguita in un sito attivo, viene visualizzata una voce che consente di registrare la visita della pagina.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Sito di Google Analytics](https://www.google.com/analytics/)
- [Sito Web Analytics Yahoo!](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Sito di StatCounter](http://statcounter.com/)
