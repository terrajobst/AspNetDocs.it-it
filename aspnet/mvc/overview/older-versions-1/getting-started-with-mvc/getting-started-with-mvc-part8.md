---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Aggiunta di una colonna al modello | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543582"
---
# <a name="adding-a-column-to-the-model"></a>Aggiunta di una colonna al modello

di [Scott hanseln](https://github.com/shanselman)

> Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Verrà creata una semplice applicazione Web che legge e scrive da un database. Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.

In questa sezione verrà illustrato in dettaglio come è possibile apportare modifiche allo schema del database e gestire le modifiche all'interno dell'applicazione.

Aggiungere una colonna "rating" alla tabella Movie. Tornare all'IDE e fare clic sull'Esplora database. Fare clic con il pulsante destro del mouse sulla tabella Movie e selezionare Apri definizione tabella.

Aggiungere una colonna "classificazione" come illustrato di seguito. Poiché non sono ora disponibili valutazioni, la colonna può consentire valori null. Fare clic su Salva.

[![la modifica della tabella Movies](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Tornare quindi al Esplora soluzioni e aprire il file Movies. edmx (che si trova nella cartella \Models). Fare clic con il pulsante destro del mouse sull'area di progettazione (l'area bianca) e selezionare Aggiorna modello da database.

[![Movies-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Verrà avviata l'"aggiornamento guidato". Fare clic sulla scheda Aggiorna al suo interno e fare clic su fine. La classe del modello di film verrà aggiornata con la nuova colonna.

![Aggiornamento guidato (2)](getting-started-with-mvc-part8/_static/image5.png)

Dopo aver fatto clic su fine, è possibile vedere che la nuova colonna rating è stata aggiunta all'entità Movie nel modello.

[![entità Movie](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

È stata aggiunta una colonna nel modello di database, ma le visualizzazioni non lo conoscono.

## <a name="update-views-with-model-changes"></a>Aggiornare le visualizzazioni con modifiche al modello

È possibile aggiornare i modelli di visualizzazione in modo da riflettere la nuova colonna rating. Poiché le visualizzazioni sono state create tramite la finestra di dialogo Aggiungi visualizzazione, è possibile eliminarle e ricrearle. Tuttavia, in genere le persone hanno già apportato modifiche ai modelli di visualizzazione dalla generazione iniziale con impalcatura e vorranno aggiungere o eliminare i campi manualmente, esattamente come è stato fatto con il campo ID per crea.

Aprire il modello \Views\Movies\Index.aspx e aggiungere una classificazione &lt;&gt;&lt;/TH&gt; all'inizio della tabella Movie. Ho aggiunto il mio dopo Genre. Quindi, nella stessa posizione della colonna ma in basso, aggiungere una riga per restituire la nuova classificazione.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Il modello finale index. aspx sarà simile al seguente:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Aprire quindi il modello \Views\Movies\Create.aspx e aggiungere un'etichetta e una casella di testo per la nuova proprietà rating:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Il modello di creazione. aspx finale sarà simile al seguente e modificheremo il titolo del browser e il secondario &lt;H2&gt; titolo a qualcosa come "creare un film" mentre ci siamo qui.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Eseguire l'app e ora è disponibile un nuovo campo nel database che è stato aggiunto alla pagina Create (Crea). Aggiungere un nuovo film: questa volta con una valutazione e fare clic su Crea.

[![creare un film-Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Dopo aver fatto clic su Crea, si viene inviati alla pagina di indice in cui il nuovo film è elencato con la nuova colonna rating nel database

[Elenco di film ![-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Questa esercitazione di base è stata avviata per creare i controller, associarli alle visualizzazioni e passare i dati hardcoded. È stato quindi creato e progettato un database in cui sono stati inseriti alcuni dati. I dati sono stati recuperati dal database e visualizzati in una tabella HTML. È stato quindi aggiunto un modulo di creazione che consente all'utente di aggiungere i dati al database dall'interno dell'applicazione Web. È stata aggiunta la convalida, quindi la convalida usa JavaScript sul lato client. Infine, è stato modificato il database per includere una nuova colonna di dati, quindi sono state aggiornate le due pagine per creare e visualizzare questi nuovi dati.

A questo punto, si consiglia di passare all'esercitazione di livello intermedio "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", nonché ai numerosi video e risorse disponibili in [https://asp.net/mvc](https://asp.net/mvc) per saperne di più su ASP.NET MVC.

Buona lettura.

- Scott Hanseln- [http://hanselman.com](http://hanselman.com) e [@shanselman](http://twitter.com/shanselman) su Twitter.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part7.md)
