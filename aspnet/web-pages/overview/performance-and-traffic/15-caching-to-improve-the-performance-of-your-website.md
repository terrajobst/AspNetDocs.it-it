---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Memorizzazione dei dati nella cache in un sito di Pagine Web ASP.NET (Razor) per ottenere prestazioni migliori | Microsoft Docs
author: Rick-Anderson
description: È possibile velocizzare il sito Web con l'archivio it, ovvero la cache, ovvero i risultati dei dati che in genere richiederanno molto tempo per recuperare o elaborare un...
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 01796d3ca699a6af5d9162b22a926551435c2040
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641519"
---
# <a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Memorizzazione dei dati nella cache in un sito di Pagine Web ASP.NET (Razor) per ottenere prestazioni migliori

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come usare un helper per memorizzare nella cache le informazioni per ottenere prestazioni più veloci in un sito Web di Pagine Web ASP.NET (Razor). È possibile velocizzare il sito Web con l'archivio &#8212; it, memorizzando &#8212; nella cache i risultati dei dati che in genere richiederanno molto tempo per il recupero o l'elaborazione e che non cambiano spesso.
> 
> **Cosa si apprenderà:** 
> 
> - Come usare la memorizzazione nella cache per migliorare la velocità di risposta del sito Web.
> 
> Queste sono le funzionalità di ASP.NET introdotte nell'articolo:
> 
> - Helper `WebCache`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2.

Ogni volta che un utente richiede una pagina dal sito, il server Web deve eseguire alcune operazioni per poter soddisfare la richiesta. Per alcune pagine, il server potrebbe dover eseguire attività che accettano (comparativamente) molto tempo, ad esempio il recupero di dati da un database. Anche se queste attività non richiedono molto tempo in termini assoluti, se il sito presenta una grande quantità di traffico, un'intera serie di richieste singole che fanno sì che il server Web esegua l'attività complicata o lenta può aggiungere molto lavoro. Questo può influire negativamente sulle prestazioni del sito.

Un modo per migliorare le prestazioni del sito Web in circostanze come questa è la memorizzazione dei dati nella cache. Se il sito riceve richieste ripetute per le stesse informazioni e non è necessario modificare le informazioni per ogni persona e non è sensibile al tempo, anziché rieseguire il recupero o il ricalcolo, è possibile recuperare i dati una volta e quindi archiviare i risultati. Al successivo arrivo di una richiesta per tali informazioni, è sufficiente estrarla dalla cache.

In generale, si memorizzano nella cache le informazioni che non cambiano di frequente. Quando si inseriscono le informazioni nella cache, questa viene archiviata in memoria sul server Web. È possibile specificare la durata della memorizzazione nella cache, da secondi a giorni. Al termine del periodo di memorizzazione nella cache, le informazioni vengono rimosse automaticamente dalla cache.

> [!NOTE]
> Le voci della cache potrebbero essere rimosse per motivi diversi dalla scadenza. Ad esempio, è possibile che il server Web venga temporaneamente esaurito in memoria e un modo per recuperare la memoria è la generazione di voci dalla cache. Come si vedrà, anche se sono state inserite informazioni nella cache, è necessario verificare che siano ancora disponibili quando necessario.

Si supponga che il sito Web abbia una pagina che visualizza la temperatura e le previsioni meteorologiche correnti. Per ottenere questo tipo di informazioni, è possibile inviare una richiesta a un servizio esterno. Poiché queste informazioni non cambiano molto (ad esempio, in un periodo di due ore) e poiché le chiamate esterne richiedono tempo e larghezza di banda, è un buon candidato per la memorizzazione nella cache.

## <a name="adding-caching-to-a-page"></a>Aggiunta della memorizzazione nella cache a una pagina

ASP.NET include un helper `WebCache` che semplifica l'aggiunta di Caching al sito e l'aggiunta di dati alla cache. In questa procedura verrà creata una pagina che memorizza nella cache l'ora corrente. Questo non è un esempio reale, dal momento che l'ora corrente è qualcosa che cambia spesso e che non è più complesso da calcolare. Tuttavia, si tratta di un modo efficace per illustrare il funzionamento della memorizzazione nella cache.

1. Aggiungere una nuova pagina denominata *WebCache. cshtml* al sito Web.
2. Aggiungere il codice e il markup seguenti alla pagina:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Quando si memorizzano nella cache i dati, è possibile inserirli nella cache usando un nome univoco nel sito Web. In questo caso, si userà una voce della cache denominata `CachedTime`. Questa è la `cacheItemKey` illustrata nell'esempio di codice.

    Il codice legge prima la voce della cache `CachedTime`. Se viene restituito un valore, ovvero se la voce della cache non è null, il codice imposta semplicemente il valore della variabile temporale sui dati della cache.

    Tuttavia, se la voce della cache non esiste (ovvero è null), il codice imposta il valore di ora, lo aggiunge alla cache e imposta un valore di scadenza su un minuto. Dopo un minuto, la voce della cache viene eliminata. (Il valore di scadenza predefinito per un elemento nella cache è 20 minuti). Il comando `WebCache.Set(cacheItemKey, time, 1, false)` Mostra come aggiungere il valore dell'ora corrente alla cache e impostare la relativa scadenza su 1 minuto. L'impostazione del parametro *slidingExpiration* su `false` indica che l'ora di scadenza non viene rinnovata ogni volta che viene richiesta. Scadrà esattamente 1 minuto dopo che è stato originariamente aggiunto alla cache. Se si imposta questo valore su `true` l'ora di scadenza di 1 minuto viene reimpostata ogni volta che il valore viene richiesto dalla cache.

    Questo codice illustra il modello da usare sempre quando si memorizzano nella cache i dati. Prima di ottenere un elemento dalla cache, verificare sempre prima se il metodo `WebCache.Get` ha restituito null. Tenere presente che la voce della cache potrebbe essere scaduta o potrebbe essere stata rimossa per altri motivi, pertanto qualsiasi voce specificata non sarà mai garantita nella cache.
3. Eseguire *WebCache. cshtml* in un browser. Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla. La prima volta che si richiede la pagina, i dati temporali non sono nella cache e il codice deve aggiungere il valore di ora alla cache.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Aggiornare *WebCache. cshtml* nel browser. Questa volta, i dati temporali sono nella cache. Si noti che l'ora non è stata modificata dall'ultima volta in cui è stata visualizzata la pagina.

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Attendere un minuto prima che la cache venga svuotata, quindi aggiornare la pagina. La pagina indica di nuovo che i dati relativi all'ora non sono stati trovati nella cache e l'ora aggiornata viene aggiunta alla cache.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Visualizzazione dei dati in un grafico](https://go.microsoft.com/fwlink/?LinkId=202895)
- Informazioni di [riferimento sulle API WebCache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
