---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Creazione di URL leggibili nei siti Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo descrive il routing in un sito Web di Pagine Web ASP.NET (Razor) e in che modo è possibile usare URL più leggibili e migliori per la SEO. Che cosa sarà...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628394"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Creazione di URL leggibili nei siti Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive il routing in un sito Web di Pagine Web ASP.NET (Razor) e in che modo è possibile usare URL più leggibili e migliori per la SEO.
> 
> Contenuto dell'esercitazione:
> 
> - In che modo ASP.NET usa il routing per consentire l'uso di URL più leggibili e ricercabili.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2.

## <a name="about-routing"></a>Informazioni sul routing

Gli URL per le pagine del sito possono avere un effetto sul funzionamento del sito. Un URL &quot;&quot; intuitivo può semplificare l'uso del sito da parte degli utenti. Può anche essere utile per l'ottimizzazione del motore di ricerca (SEO) per il sito. I siti Web ASP.NET includono la possibilità di usare automaticamente URL brevi.

ASP.NET consente di creare URL significativi che descrivono le azioni dell'utente anziché semplicemente puntare a un file sul server. Prendere in considerazione questi URL per un Blog fittizio:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Confrontare questi URL con quelli seguenti:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

Nella prima coppia, è necessario che un utente sappia che il Blog viene visualizzato usando la pagina *Blog. cshtml* e quindi deve creare una stringa di query che ottenga la categoria corretta o l'intervallo di date. Il secondo set di esempi è molto più semplice da comprendere e creare.

Gli URL per il primo esempio puntano anche direttamente a un file specifico (*Blog. cshtml*). Se per qualche motivo il Blog è stato spostato in un'altra cartella del server o se il Blog è stato riscritto in modo da usare una pagina diversa, i collegamenti potrebbero non essere corretti. Il secondo set di URL non punta a una pagina specifica, quindi anche se l'implementazione del Blog o la posizione cambiano, gli URL sarebbero ancora validi.

In Pagine Web ASP.NET, è possibile creare URL più amichevoli come quelli degli esempi precedenti, perché ASP.NET usa il *routing*. Il routing crea il mapping logico da un URL a una pagina o pagine che può soddisfare la richiesta. Poiché il mapping è logico (non fisico, a un file specifico), il routing garantisce una grande flessibilità per la definizione degli URL per il sito.

## <a name="how-routing-works"></a>Funzionamento del routing

Quando ASP.NET elabora una richiesta, legge l'URL per determinare la modalità di routing. ASP.NET tenta di trovare la corrispondenza dei singoli segmenti dell'URL con i file su disco, passando da sinistra a destra. Se si verifica una corrispondenza, qualsiasi elemento rimanente nell'URL viene passato alla pagina come *informazioni sul percorso*.

Si supponga che un utente effettua una richiesta utilizzando questo URL:

`http://www.contoso.com/a/b/c`

La ricerca sarà simile alla seguente:

1. È presente un file con il percorso e il nome di */a/b/c.cshtml*? In tal caso, eseguire la pagina e non passarvi alcuna informazione. In caso contrario...
2. È presente un file con il percorso e il nome di */a/b.cshtml*? In tal caso, eseguire la pagina e passare il valore `c`. In caso contrario...
3. È presente un file con il percorso e il nome di */a.cshtml*? In tal caso, eseguire la pagina e passare il valore `b/c`.

Se la ricerca non ha rilevato corrispondenze esatte per i file con *estensione cshtml* nelle cartelle specificate, ASP.NET continua a cercare questi file a turno:

1. */a/b/c/default.cshtml* (nessuna informazione sul percorso).
2. */a/b/c/index.cshtml* (nessuna informazione sul percorso).

> [!NOTE]
> Per chiarire, le richieste di pagine specifiche, ovvero le richieste che includono l'estensione del nome file *. cshtml* , funzionano esattamente come previsto. Una richiesta come `http://www.contoso.com/a/b.cshtml` eseguirà la pagina *b. cshtml* .

All'interno di una pagina, è possibile ottenere le informazioni sul percorso tramite la proprietà `UrlData` della pagina, che è un dizionario. Si supponga di avere un file denominato *ViewCustomers. cshtml* e che il sito riceva questa richiesta:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Come descritto nelle regole precedenti, la richiesta passerà alla pagina. All'interno della pagina, è possibile usare codice simile al seguente per ottenere e visualizzare le informazioni sul percorso (in questo caso, il valore &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Poiché il routing non implica nomi file completi, è possibile che si verifichino ambiguità se si dispone di pagine con lo stesso nome ma con estensioni di file diverse, ad esempio *cshtml* e *Page. html*. Per evitare problemi di routing, è consigliabile assicurarsi che nel sito non siano presenti pagine i cui nomi differiscono solo per l'estensione.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[WebMatrix: URL, UrlData e routing per SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Questo post di Blog di Mike Brind fornisce alcuni dettagli aggiuntivi sul funzionamento del routing in Pagine Web ASP.NET.
