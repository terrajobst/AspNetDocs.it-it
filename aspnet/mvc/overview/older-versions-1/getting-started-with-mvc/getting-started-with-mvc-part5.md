---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Accesso ai dati del modello da un controller | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543750"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Accesso ai dati del modello da un controller

di [Scott hanseln](https://github.com/shanselman)

> Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Verrà creata una semplice applicazione Web che legge e scrive da un database. Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.

In questa sezione si creerà una nuova classe MoviesController e si scriverà il codice che recupera i dati dei film e li visualizza nuovamente nel browser usando un modello di visualizzazione.

Fare clic con il pulsante destro del mouse sulla cartella Controllers e creare un nuovo MoviesController.

[![Aggiungi controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Verrà creato un nuovo file "MoviesController.cs" sotto la cartella \Controllers all'interno del progetto. Aggiornare MovieController per recuperare l'elenco di filmati dal database appena popolato.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Viene eseguita una query LINQ per recuperare solo i film rilasciati dopo l'estate del 1984. È necessario un modello di visualizzazione per eseguire il rendering di questo elenco di filmati, quindi fare clic con il pulsante destro del mouse sul metodo e scegliere Aggiungi visualizzazione per crearlo.

Nella finestra di dialogo Aggiungi visualizzazione si indicherà che verrà passato un elenco&lt;Movies. Models. Movie&gt; al modello di visualizzazione. Diversamente dalle volte precedenti è stata usata la finestra di dialogo Aggiungi visualizzazione e si è scelto di creare un modello "vuoto", questa volta indicheremo che Visual Studio dovrà eseguire automaticamente il "patibolo" di un modello di visualizzazione per Microsoft con un contenuto predefinito. Questa operazione verrà eseguita selezionando l'elemento "list" nel menu a discesa Visualizza contenuto.

Tenere presente che, quando è stata creata una nuova classe, è necessario compilare l'applicazione affinché venga visualizzata nella finestra di dialogo Aggiungi visualizzazione.

![Aggiungi visualizzazione](getting-started-with-mvc-part5/_static/image3.png)

Fare clic su Aggiungi e il sistema genererà automaticamente il codice per una vista che Visualizza l'elenco dei filmati. Questo è il momento giusto per modificare l'intestazione &lt;H2&gt; su un valore simile a "My Movie List", come in precedenza con la visualizzazione Hello World.

[![Movies-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Eseguire l'applicazione e visitare/Movies nella barra degli indirizzi. Ora che sono stati recuperati i dati dal database usando una query di base all'interno del controller e i dati sono stati restituiti a una vista che conosce i film. Tale visualizzazione consente quindi di scorrere l'elenco dei film e di creare una tabella di dati per noi.

[Elenco di film ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Non verranno implementate le funzionalità di modifica, dettagli ed eliminazione con questa applicazione, quindi non sono necessari i collegamenti predefiniti creati dal modello di impalcatura. Aprire il file/Movies/Index.aspx e rimuoverli.

Ecco il codice sorgente per il modello di vista aggiornato che dovrebbe essere simile dopo aver apportato queste modifiche:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Si tratta di creare collegamenti che non saranno necessari, quindi verranno eliminati per questo esempio. Tuttavia, verrà mantenuto il collegamento Crea nuovo. Di seguito è riportato l'aspetto dell'app con la colonna rimossa.

[Elenco di film ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

È ora disponibile un semplice elenco di dati dei film. Tuttavia, se si fa clic sul collegamento "Crea nuovo", viene ricevuto un errore perché non è collegato. Viene ora implementato un metodo di azione create che consente a un utente di immettere nuovi film nel database.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part4.md)
> [Successivo](getting-started-with-mvc-part6.md)
