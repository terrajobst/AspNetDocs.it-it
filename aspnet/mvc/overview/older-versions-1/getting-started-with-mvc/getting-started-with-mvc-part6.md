---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Aggiunta di un metodo di creazione e di una visualizzazione | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 05a281720f76b107fe8d902ef60d5d2e72af3ef5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543659"
---
# <a name="adding-a-create-method-and-create-view"></a>Aggiunta di un metodo di creazione e di una visualizzazione di creazione

di [Scott hanseln](https://github.com/shanselman)

> Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Verrà creata una semplice applicazione Web che legge e scrive da un database. Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.

In questa sezione verrà implementato il supporto necessario per consentire agli utenti di creare nuovi film nel database. Questa operazione verrà eseguita implementando l'azione dell'URL/Movies/Create.

L'implementazione dell'URL di/Movies/Create è un processo in due passaggi. Quando un utente visita per la prima volta l'URL di/Movies/Create, si vuole visualizzare un modulo HTML che può compilare per immettere un nuovo film. Quindi, quando l'utente invia il modulo e invia i dati al server, si vuole recuperare il contenuto inserito e salvarlo nel database.

Questi due passaggi verranno implementati all'interno di due metodi Create () all'interno della classe MoviesController. Un metodo visualizzerà il modulo &lt;&gt; che l'utente deve compilare per creare un nuovo film. Il secondo metodo gestirà l'elaborazione dei dati inviati quando l'utente invia il modulo di &lt;&gt; di nuovo al server e salva un nuovo film nel database.

Di seguito è riportato il codice da aggiungere alla classe MoviesController per implementarlo:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Il codice precedente contiene tutto il codice necessario all'interno del controller.

A questo punto, è possibile implementare il modello Create View che verrà usato per visualizzare un modulo per l'utente. Fare clic con il pulsante destro del mouse sul primo metodo create e selezionare "Aggiungi visualizzazione" per creare il modello di visualizzazione per il form di film.

Selezioniamo che verrà passato il modello di vista a "film" come classe di dati di visualizzazione e indicheremo che vogliamo "impalcature" un modello di "creazione".

[![Aggiungi visualizzazione](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Dopo aver fatto clic sul pulsante Aggiungi, verrà creato automaticamente il modello di visualizzazione \Movies\Create.aspx. Poiché è stata selezionata l'opzione "Crea" nell'elenco a discesa "Visualizza contenuto", la finestra di dialogo Aggiungi visualizzazione aggiunge automaticamente un contenuto predefinito per l'utente. L'impalcatura ha creato un modulo HTML &lt;&gt;, un posto per i messaggi di errore di convalida e, dal momento che l'impalcatura conosce i film, ha creato etichette e campi per ogni proprietà della classe.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Poiché il database assegna automaticamente un ID a un film, rimuovere i campi che fanno riferimento a Model. ID della vista di creazione. Rimuovere le 7 righe dopo &lt;campi&gt;legenda&lt;/Legend&gt; perché visualizzano il campo ID che non si desidera.

A questo punto, creare un nuovo film e aggiungerlo al database. Per eseguire questa operazione, eseguire di nuovo l'applicazione e visitare l'URL "/Movies" e fare clic sul collegamento "Crea" per aggiungere un nuovo film.

[![creare-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Quando si fa clic sul pulsante Create (Crea), verrà eseguito il postback (tramite HTTP POST) i dati in questo modulo al metodo/Movies/Create appena creato. Proprio come quando il sistema ha automaticamente accettato il parametro "numTimes" e "Name" dall'URL e ne è stato eseguito il mapping ai parametri in un metodo precedente, il sistema prenderà automaticamente i campi del modulo da un POST e ne eseguirà il mapping a un oggetto. In questo caso, i valori dei campi in HTML come "ReleaseDate" e "title" verranno inseriti automaticamente nelle proprietà corrette di una nuova istanza di un film.

Esaminiamo di nuovo il secondo metodo create da MoviesController. Si noti come un oggetto "Movie" viene considerato come un argomento:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Questo oggetto Movie è stato quindi passato alla versione [HttpPost] del metodo di azione create e salvato nel database e quindi reindirizzato l'utente al metodo di azione index () che visualizzerà il risultato salvato nell'elenco dei film:

[Elenco di film ![-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Non viene verificato se i film sono corretti, ma il database non ci permette di salvare un film senza titolo. Sarebbe bello se avessimo potuto informare l'utente che prima che il database generasse un errore. Questa operazione verrà eseguita successivamente aggiungendo il supporto della convalida all'applicazione.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part5.md)
> [Successivo](getting-started-with-mvc-part7.md)
