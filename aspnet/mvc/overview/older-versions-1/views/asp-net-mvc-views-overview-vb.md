---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: Panoramica delle visualizzazioni MVC ASP.NET (VB) | Microsoft Docs
author: StephenWalther
description: Che cos'è una visualizzazione MVC ASP.NET e in che modo differisce da una pagina HTML? In questa esercitazione Stephen Walther presenta le visualizzazioni e Mostra come è possibile...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f02728ed248f29b09d654e509977ed43889cbb83
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541307"
---
# <a name="aspnet-mvc-views-overview-vb"></a>Panoramica delle visualizzazioni ASP.NET MVC (VB)

di [Stephen Walther](https://github.com/StephenWalther)

> Che cos'è una visualizzazione MVC ASP.NET e in che modo differisce da una pagina HTML? In questa esercitazione, Stephen Walther presenta le visualizzazioni e illustra come sfruttare i vantaggi offerti da Visualizza dati e helper HTML in una visualizzazione.

Lo scopo di questa esercitazione è fornire una breve introduzione alle viste MVC ASP.NET, alla visualizzazione dei dati e agli helper HTML. Al termine di questa esercitazione, è necessario comprendere come creare nuove visualizzazioni, passare dati da un controller a una vista e utilizzare gli helper HTML per generare contenuto in una visualizzazione.

## <a name="understanding-views"></a>Informazioni sulle viste

A differenza di ASP.NET o Active Server Pages, ASP.NET MVC non include alcun elemento che corrisponda direttamente a una pagina. In un'applicazione MVC ASP.NET non è presente una pagina su disco corrispondente al percorso nell'URL digitato nella barra degli indirizzi del browser. L'elemento più vicino a una pagina in un'applicazione MVC ASP.NET è un elemento denominato *visualizzazione*.

In un'applicazione MVC ASP.NET, le richieste del browser in ingresso sono mappate alle azioni del controller. Un'azione del controller può restituire una visualizzazione. Tuttavia, un'azione del controller può eseguire un altro tipo di azione, ad esempio il reindirizzamento a un'altra azione del controller.

Il listato 1 contiene un semplice controller denominato HomeController. HomeController espone due azioni controller denominate index () e Details ().

**Listato 1-HomeController. vb**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

È possibile richiamare la prima azione, ovvero l'azione index (), digitando l'URL seguente nella barra degli indirizzi del browser:

/Home/Index

È possibile richiamare la seconda azione, ovvero l'azione dettagli (), digitando questo indirizzo nel browser:

/Home/Details

L'azione index () restituisce una visualizzazione. La maggior parte delle azioni create restituirà le visualizzazioni. Un'azione può tuttavia restituire altri tipi di risultati dell'azione. Ad esempio, l'azione Details () restituisce un RedirectToActionResult che reindirizza la richiesta in ingresso all'azione index ().

L'azione index () contiene la singola riga di codice seguente:

Visualizza ()

Questa riga di codice restituisce una vista che deve trovarsi nel percorso seguente del server Web:

\Views\Home\Index.aspx

Il percorso della visualizzazione viene dedotto dal nome del controller e dal nome dell'azione del controller.

Se si preferisce, è possibile essere espliciti sulla visualizzazione. La riga di codice seguente restituisce una visualizzazione denominata Fred:

Visualizzazione (Fred)

Quando viene eseguita questa riga di codice, viene restituita una vista dal percorso seguente:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Se si prevede di creare unit test per l'applicazione ASP.NET MVC, è consigliabile essere espliciti sui nomi delle viste. In questo modo, è possibile creare un unit test per verificare che la vista prevista sia stata restituita da un'azione del controller.

## <a name="adding-content-to-a-view"></a>Aggiunta di contenuto a una visualizzazione

Una vista è un documento HTML standard (X) che può contenere script. Usare gli script per aggiungere contenuto dinamico a una visualizzazione.

Ad esempio, la vista nel listato 2 Visualizza la data e l'ora correnti.

**Listato 2-\Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

Si noti che il corpo della pagina HTML nel listato 2 contiene lo script seguente:

&lt;% Response. Write (DateTime. Now)%&gt;

Per contrassegnare l'inizio e la fine di uno script, è possibile utilizzare i delimitatori di script &lt;% e%&gt;. Questo script è scritto in Visual Basic. Visualizza la data e l'ora correnti chiamando il metodo Response. Write () per eseguire il rendering del contenuto nel browser. È possibile utilizzare i delimitatori di script &lt;% e%&gt; per eseguire una o più istruzioni.

Poiché si chiama Response. Write () in modo frequente, Microsoft fornisce un collegamento per chiamare il metodo Response. Write (). La vista nel listato 3 USA i delimitatori &lt;% = e%&gt; come collegamento per chiamare Response. Write ().

**Listato 3-Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

È possibile utilizzare qualsiasi linguaggio .NET per generare contenuto dinamico in una vista. In genere, si userà Visual Basic .NET o C# per scrivere i controller e le visualizzazioni.

## <a name="using-html-helpers-to-generate-view-content"></a>Uso di helper HTML per generare il contenuto della visualizzazione

Per semplificare l'aggiunta di contenuto a una visualizzazione, è possibile utilizzare una funzionalità denominata *helper HTML*. Un helper HTML, in genere, è un metodo che genera una stringa. È possibile usare gli helper HTML per generare elementi HTML standard quali caselle di testo, collegamenti, elenchi a discesa e caselle di riepilogo.

Ad esempio, la vista nel listato 4 sfrutta i vantaggi di tre helper HTML, ovvero gli helper BeginForm (), TextBox () e password () per generare un form di accesso (vedere la figura 1).

**Listato 4--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]

[![finestra di dialogo nuovo progetto](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)

**Figura 01**: form di accesso standard ([fare clic per visualizzare l'immagine con dimensioni complete](asp-net-mvc-views-overview-vb/_static/image2.png))

Tutti i metodi helper HTML vengono chiamati sulla proprietà HTML della vista. Ad esempio, si esegue il rendering di una casella di testo chiamando il metodo HTML. TextBox ().

Si noti che è possibile utilizzare i delimitatori di script &lt;% = e%&gt; quando si chiamano gli helper HTML. TextBox () e HTML. password (). Questi helper restituiscono semplicemente una stringa. È necessario chiamare Response. Write () per eseguire il rendering della stringa nel browser.

L'uso di metodi helper HTML è facoltativo. Semplificano la vita riducendo la quantità di codice HTML e script che è necessario scrivere. La visualizzazione nel listato 5 esegue il rendering dello stesso formato della vista nel listato 4 senza usare gli helper HTML.

**Listato 5--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

È anche possibile creare gli helper HTML. Ad esempio, è possibile creare un metodo helper GridView () che visualizza automaticamente un set di record di database in una tabella HTML. Questo argomento viene illustrato nell'esercitazione **creazione di helper HTML personalizzati**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Utilizzo di Visualizza dati per passare dati a una vista

È possibile utilizzare Visualizza dati per passare dati da un controller a una vista. Si pensi ai dati di visualizzazione, ad esempio un pacchetto inviato tramite posta elettronica. Tutti i dati passati da un controller a una vista devono essere inviati utilizzando questo pacchetto. Ad esempio, il controller nel listato 6 aggiunge un messaggio per visualizzare i dati.

**Elenco 6-ProductController. vb**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

La proprietà Controller ViewData rappresenta una raccolta di coppie nome/valore. Nel listato 6 il metodo index () aggiunge un elemento alla raccolta di dati View denominata Message con il valore Hello World!. Quando la vista viene restituita dal metodo index (), i dati della visualizzazione vengono passati automaticamente alla visualizzazione.

La vista nel listato 7 recupera il messaggio dai dati della vista e ne esegue il rendering nel browser.

**Listato 7--\Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

Si noti che la vista sfrutta il metodo di supporto HTML HTML. Encode () durante il rendering del messaggio. L'helper HTML HTML. Encode () codifica i caratteri speciali, ad esempio &lt; e &gt; in caratteri che possono essere visualizzati in modo sicuro in una pagina Web. Quando si esegue il rendering del contenuto inviato da un utente a un sito Web, è necessario codificare il contenuto per impedire attacchi intrusivi in JavaScript.

Poiché il messaggio è stato creato in ProductController, non è necessario codificare il messaggio. Tuttavia, è consigliabile chiamare sempre il metodo HTML. Encode () quando viene visualizzato contenuto recuperato dalla vista dati all'interno di una vista.

Nel listato 7 sono stati usati i dati di visualizzazione per passare un semplice messaggio stringa da un controller a una vista. È anche possibile usare Visualizza dati per passare altri tipi di dati, ad esempio una raccolta di record di database, da un controller a una vista. Se, ad esempio, si desidera visualizzare il contenuto della tabella di database Products in una vista, è necessario passare la raccolta dei record di database in View Data.

È anche possibile passare dati di visualizzazione fortemente tipizzati da un controller a una vista. Si esplorerà questo argomento nell'esercitazione **informazioni sulle visualizzazioni e i dati fortemente tipizzati**.

## <a name="summary"></a>Riepilogo

Questa esercitazione ha fornito una breve introduzione alle viste MVC ASP.NET, alla visualizzazione dei dati e agli helper HTML. Nella prima sezione si è appreso come aggiungere nuove visualizzazioni al progetto. Si è appreso che è necessario aggiungere una visualizzazione alla cartella corretta per poterla chiamare da un particolare controller. Successivamente, è stato illustrato l'argomento degli helper HTML. Si è appreso come gli helper HTML consentono di generare facilmente contenuto HTML standard. Infine, si è appreso come sfruttare i vantaggi dei dati di visualizzazione per passare dati da un controller a una vista.

> [!div class="step-by-step"]
> [Precedente](passing-data-to-view-master-pages-cs.md)
> [Successivo](creating-custom-html-helpers-vb.md)
