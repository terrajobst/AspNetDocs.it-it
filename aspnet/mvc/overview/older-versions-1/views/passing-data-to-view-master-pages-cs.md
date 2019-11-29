---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: Passaggio di dati a pagine master diC#visualizzazione () | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è spiegare come è possibile passare i dati da un controller a una pagina master di visualizzazione. Vengono esaminate due strategie per il passaggio dei dati a una vista m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1492175812b0a092cd1594a770e348efe9b4122b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74593799"
---
# <a name="passing-data-to-view-master-pages-c"></a>Passaggio di dati a pagine master di visualizzazione (C#)

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> L'obiettivo di questa esercitazione è spiegare come è possibile passare i dati da un controller a una pagina master di visualizzazione. Vengono esaminate due strategie per il passaggio dei dati a una pagina master di visualizzazione. In primo luogo, viene illustrata una soluzione semplice che consente di ottenere un'applicazione difficile da gestire. Viene quindi esaminata una soluzione molto migliore che richiede un lavoro iniziale più semplice, ma comporta un'applicazione molto più gestibile.

## <a name="passing-data-to-view-master-pages"></a>Passaggio di dati a pagine master di visualizzazione

L'obiettivo di questa esercitazione è spiegare come è possibile passare i dati da un controller a una pagina master di visualizzazione. Vengono esaminate due strategie per il passaggio dei dati a una pagina master di visualizzazione. In primo luogo, viene illustrata una soluzione semplice che consente di ottenere un'applicazione difficile da gestire. Viene quindi esaminata una soluzione molto migliore che richiede un lavoro iniziale più semplice, ma comporta un'applicazione molto più gestibile.

### <a name="the-problem"></a>Il problema

Si supponga di creare un'applicazione di database di film e di voler visualizzare l'elenco delle categorie di film in ogni pagina dell'applicazione (vedere la figura 1). Si supponga inoltre che l'elenco di categorie di film sia archiviato in una tabella di database. In tal caso, sarebbe opportuno recuperare le categorie dal database ed eseguire il rendering dell'elenco di categorie di film all'interno di una pagina di visualizzazione master.

[![la visualizzazione delle categorie di film in una pagina di visualizzazione Master](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**Figura 01**: visualizzazione delle categorie di film in una pagina di visualizzazione Master ([fare clic per visualizzare l'immagine con dimensioni complete](passing-data-to-view-master-pages-cs/_static/image3.png))

Ecco il problema. Come si recupera l'elenco di categorie di film nella pagina master? Si tenta di chiamare direttamente i metodi delle classi del modello nella pagina master. In altre parole, si sta tentando di includere il codice per il recupero dei dati dal database direttamente nella pagina master. Tuttavia, ignorare i controller MVC per accedere al database violerebbe la netta separazione dei problemi che rappresenta uno dei principali vantaggi della creazione di un'applicazione MVC.

In un'applicazione MVC è necessario che tutte le interazioni tra le visualizzazioni MVC e il modello MVC siano gestite dai controller MVC. Questa separazione dei problemi comporta un'applicazione più gestibile, adattabile e testabile.

In un'applicazione MVC tutti i dati passati a una vista, inclusa una pagina master di visualizzazione, devono essere passati a una vista da un'azione del controller. Inoltre, i dati devono essere passati sfruttando i dati di visualizzazione. Nella parte restante di questa esercitazione vengono esaminati due metodi per passare i dati della visualizzazione a una pagina master di visualizzazione.

### <a name="the-simple-solution"></a>Soluzione semplice

Iniziamo con la soluzione più semplice per passare i dati di visualizzazione da un controller a una pagina master di visualizzazione. La soluzione più semplice consiste nel passare i dati di visualizzazione per la pagina master in ogni azione del controller.

Si consideri il controller nel listato 1. Espone due azioni denominate `Index()` e `Details()`. Il metodo di azione `Index()` restituisce ogni film della tabella del database Movies. Il metodo di azione `Details()` restituisce ogni film in una particolare categoria di film.

**Listato 1-`Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

Si noti che le azioni index () e Details () aggiungono due elementi per visualizzare i dati. L'azione index () aggiunge due chiavi: categorie e filmati. La chiave Categories rappresenta l'elenco di categorie di film visualizzate dalla pagina master di visualizzazione. La chiave Movies rappresenta l'elenco dei film visualizzati dalla pagina di visualizzazione dell'indice.

L'azione dettagli () aggiunge anche due chiavi denominate categorie e filmati. Il tasto Categories, ancora una volta, rappresenta l'elenco delle categorie di film visualizzate dalla pagina master di visualizzazione. La chiave Movies rappresenta l'elenco dei film in una categoria specifica visualizzata dalla pagina di visualizzazione dettagli (vedere la figura 2).

[![la visualizzazione dettagli](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**Figura 02**: visualizzazione dettagli ([fare clic per visualizzare l'immagine con dimensioni complete](passing-data-to-view-master-pages-cs/_static/image6.png))

La vista index è inclusa nell'elenco 2. Scorre semplicemente l'elenco dei film rappresentati dall'elemento Movies in View Data.

**Listato 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

La pagina di visualizzazione Master è inclusa nell'elenco 3. La pagina master di visualizzazione scorre ed esegue il rendering di tutte le categorie di film rappresentate dall'elemento categorie dai dati della visualizzazione.

**Listato 3-`Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

Tutti i dati vengono passati alla visualizzazione e alla pagina master di visualizzazione tramite i dati della visualizzazione. Questo è il modo corretto per passare i dati alla pagina master.

Qual è il problema di questa soluzione? Il problema è che questa soluzione viola il principio secco (Don't Repeat Yourself). Ogni azione del controller deve aggiungere lo stesso elenco di categorie di film per visualizzare i dati. Il codice duplicato nell'applicazione rende l'applicazione molto più difficile da gestire, adattare e modificare.

### <a name="the-good-solution"></a>La soluzione migliore

In questa sezione viene esaminata una soluzione alternativa e migliore per passare i dati da un'azione del controller a una pagina master di visualizzazione. Anziché aggiungere le categorie di film per la pagina master in ogni azione del controller, le categorie di film vengono aggiunte ai dati di visualizzazione una sola volta. Tutti i dati della visualizzazione utilizzati dalla pagina master di visualizzazione vengono aggiunti in un controller dell'applicazione.

La classe ApplicationController è contenuta nel listato 4.

**Listato 4-`Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

Sono disponibili tre elementi che è necessario notare sul controller dell'applicazione nel listato 4. Si noti prima di tutto che la classe eredita dalla classe di base System. Web. Mvc. controller. Il controller dell'applicazione è una classe controller.

In secondo luogo, si noti che la classe del controller dell'applicazione è una classe astratta. Una classe astratta è una classe che deve essere implementata da una classe concreta. Poiché il controller dell'applicazione è una classe astratta, non è possibile richiamare direttamente i metodi definiti nella classe. Se si tenta di richiamare direttamente la classe dell'applicazione, si otterrà un messaggio di errore di risorsa non trovato.

Infine, si noti che il controller dell'applicazione contiene un costruttore che aggiunge l'elenco di categorie di film per visualizzare i dati. Ogni classe controller che eredita dal controller dell'applicazione chiama automaticamente il costruttore del controller dell'applicazione. Ogni volta che si chiama qualsiasi azione su qualsiasi controller che eredita dal controller dell'applicazione, le categorie di film vengono incluse automaticamente nella visualizzazione dati.

Il controller di film nel listato 5 eredita dal controller dell'applicazione.

**Elenco 5-`Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

Il controller di film, proprio come il controller Home illustrato nella sezione precedente, espone due metodi di azione denominati `Index()` e `Details()`. Si noti che l'elenco di categorie di film visualizzato nella pagina di visualizzazione master non è stato aggiunto per visualizzare i dati nel metodo `Index()` o `Details()`. Poiché il controller di film eredita dal controller dell'applicazione, viene aggiunto l'elenco di categorie di film per visualizzare automaticamente i dati.

Si noti che questa soluzione per aggiungere dati di visualizzazione per una pagina master di visualizzazione non viola il principio secco (Don't Repeat Yourself). Il codice per l'aggiunta dell'elenco di categorie di film per la visualizzazione dei dati è contenuto in una sola posizione: il costruttore per il controller dell'applicazione.

### <a name="summary"></a>Riepilogo

In questa esercitazione sono stati descritti due approcci per passare i dati di visualizzazione da un controller a una pagina master di visualizzazione. Per prima cosa, abbiamo esaminato un approccio semplice, ma difficile da gestire. Nella prima sezione è stato illustrato come è possibile aggiungere i dati di visualizzazione per una pagina master di visualizzazione in ogni azione del controller nell'applicazione. Si è concluso che questo approccio non è valido perché viola il principio secco (Don't Repeat Yourself).

Successivamente, è stata esaminata una strategia molto migliore per l'aggiunta di dati richiesti da una pagina master di visualizzazione per visualizzare i dati. Anziché aggiungere i dati della visualizzazione in ogni azione del controller, i dati di visualizzazione sono stati aggiunti solo una volta all'interno di un controller dell'applicazione. In questo modo, è possibile evitare codice duplicato quando si passano dati a una pagina master di visualizzazione in un'applicazione MVC ASP.NET.

> [!div class="step-by-step"]
> [Precedente](creating-page-layouts-with-view-master-pages-cs.md)
> [Successivo](asp-net-mvc-views-overview-vb.md)
