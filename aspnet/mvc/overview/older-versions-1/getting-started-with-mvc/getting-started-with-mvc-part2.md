---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Aggiunta di un controller | Microsoft Docs
author: shanselman
description: Una versione aggiornata se questa esercitazione è disponibile qui usando Visual Studio 2013. La nuova esercitazione USA ASP.NET MVC 5, che fornisce molti miglioramenti rispetto a t...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: e2a298584473f57c2b14edf507f0f6886d906ea3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543981"
---
# <a name="adding-a-controller"></a>Aggiunta di un controller

di [Scott hanseln](https://github.com/shanselman)

> > [!NOTE]
> > Una versione aggiornata se questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). La nuova esercitazione USA ASP.NET MVC 5, che fornisce molti miglioramenti rispetto a questa esercitazione.
>
>
> Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Verrà creata una semplice applicazione Web che legge e scrive da un database. Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.

MVC rappresenta il modello, la visualizzazione e il controller. MVC è un modello per lo sviluppo di applicazioni in modo che ogni parte abbia una responsabilità diversa da un'altra.

- Modello: i dati dell'applicazione
- Views: i file modello che l'applicazione userà per generare dinamicamente risposte HTML.
- Controller: classi che gestiscono le richieste URL in ingresso per l'applicazione, recuperano i dati del modello e quindi specificano i modelli di visualizzazione che restituiscono una risposta al client

Verranno trattati tutti questi concetti in questa esercitazione e verrà illustrato come usarli per creare un'applicazione.

Per creare un nuovo controller, fare clic con il pulsante destro del mouse sulla cartella controller in Esplora soluzioni e scegliere Aggiungi controller.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Assegnare un nome al nuovo controller "HelloWorldController" e fare clic su Aggiungi.

[![finestra di dialogo Aggiungi controller](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Si noti che nella Esplora soluzioni a destra che è stato creato un nuovo file denominato HelloWorldController.cs e che il file è ora aperto nell' **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Creare due nuovi metodi che hanno un aspetto simile all'interno della nuova classe pubblica HelloWorldController. Verrà restituita una stringa di codice HTML direttamente dal controller come esempio.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Il controller è denominato HelloWorldController e il nuovo metodo è denominato index. Eseguire di nuovo l'applicazione, esattamente come in precedenza (fare clic sul pulsante Riproduci o premere F5 per eseguire questa operazione). Al termine dell'avvio del browser, modificare il percorso nella barra degli indirizzi `http://localhost:xx/HelloWorld` dove XX corrisponde a qualsiasi numero scelto dal computer. A questo punto il browser dovrebbe avere un aspetto simile alla schermata seguente. Nel metodo precedente è stata restituita una stringa passata a un metodo denominato "Content". Abbiamo indicato che il sistema restituisce solo codice HTML ed è stato fatto.

ASP.NET MVC richiama diverse classi controller (e metodi di azione diversi al loro interno) a seconda dell'URL in ingresso. La logica di mapping predefinita usata da ASP.NET MVC usa un formato simile al seguente per controllare quale codice eseguire:

/[Controller]/[actionName]/[parametri]

La prima parte dell'URL determina la classe controller da eseguire. Quindi/HelloWorld esegue il mapping alla classe HelloWorldController. La seconda parte dell'URL determina il metodo di azione sulla classe da eseguire. Pertanto/HelloWorld/Index provocherebbe l'esecuzione del metodo index () della classe HelloWorldController. Si noti che è stato necessario solo visitare/HelloWorld e l'indice del metodo era implicito. Questo perché un metodo denominato "index" è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato uno in modo esplicito.

[![questa è l'azione predefinita](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Ora, visitiamo `http://localhost:xx/HelloWorld/Welcome.` ora che il metodo Welcome è stato eseguito e ha restituito la relativa stringa HTML.

Anche in questo caso,/[controller]/[actionName]/[Parameters], quindi il controller è HelloWorld e Welcome è il metodo. Non sono ancora stati eseguiti parametri.

[![questo è il metodo di azione iniziale](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Modificare leggermente l'esempio in modo che sia possibile passare alcune informazioni dall'URL al controller, ad esempio:/HelloWorld/Welcome? Name = Scott&amp;numtimes = 4. Modificare il metodo Welcome per includere due parametri e aggiornarlo come indicato di seguito. Si noti che è stata usata C# la funzionalità facoltativa del parametro per indicare che il parametro numTimes deve essere impostato su 1 se non viene passato.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Eseguire l'applicazione e visitare `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` modificando il valore di Name e numtimes come si desidera. Il sistema ha automaticamente eseguito il mapping dei parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.

In entrambi questi esempi il controller ha eseguito tutto il lavoro e ha restituito direttamente HTML. In genere non si vuole che i controller restituiscano direttamente HTML, perché il codice è molto complesso. Si userà in genere un file modello di vista separato per facilitare la generazione della risposta HTML. Esaminiamo ora come possiamo eseguire questa operazione. Chiudere il browser e tornare all'IDE.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part1.md)
> [Successivo](getting-started-with-mvc-part3.md)
