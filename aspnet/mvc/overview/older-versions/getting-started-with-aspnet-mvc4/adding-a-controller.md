---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Aggiunta di un controller | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f528c56435976c7f31fce453c834ef9eaebe6244
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78599841"
---
# <a name="adding-a-controller"></a>Aggiunta di un controller

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.

MVC è l'acronimo di *Model-View-Controller*. MVC è un modello per lo sviluppo di applicazioni ben progettate, verificabili e facili da gestire. Le applicazioni basate su MVC contengono:

- **M** Odelli: classi che rappresentano i dati dell'applicazione e che utilizzano la logica di convalida per applicare le regole business per tali dati.
- **V** iste: file modello usati dall'applicazione per generare dinamicamente risposte HTML.
- **C** ontroller: classi che gestiscono le richieste del browser in ingresso, recuperano i dati del modello e quindi specificano i modelli di visualizzazione che restituiscono una risposta al browser.

Verranno trattati tutti questi concetti in questa serie di esercitazioni e verrà illustrato come usarli per creare un'applicazione.

Iniziamo creando una classe controller. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *controller* e quindi scegliere **Aggiungi controller**.

![](adding-a-controller/_static/image1.png)

Assegnare un nome al nuovo controller &quot;HelloWorldController&quot;. Lasciare il modello predefinito come **controller MVC vuoto** e fare clic su **Aggiungi**.

![Aggiungi controller](adding-a-controller/_static/image2.png)

Si noti che in **Esplora soluzioni** è stato creato un nuovo file denominato *HelloWorldController.cs*. Il file è aperto nell'IDE.

![](adding-a-controller/_static/image3.png)

Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

I metodi del controller restituiranno una stringa di codice HTML come esempio. Il controller è denominato `HelloWorldController` e il primo metodo precedente è denominato `Index`. Che verrà ora richiamato da un browser. Eseguire l'applicazione (premere F5 o CTRL + F5). Nel browser aggiungere &quot;HelloWorld&quot; al percorso nella barra degli indirizzi. Nell'illustrazione seguente, ad esempio, è `http://localhost:1234/HelloWorld.`) La pagina nel browser sarà simile alla schermata seguente. Nel metodo precedente, il codice ha restituito direttamente una stringa. Si è detto al sistema di restituire solo codice HTML,

![](adding-a-controller/_static/image4.png)

ASP.NET MVC richiama diverse classi controller (e metodi di azione diversi al loro interno) a seconda dell'URL in ingresso. La logica di routing degli URL predefinita usata da ASP.NET MVC usa un formato simile al seguente per determinare il codice da richiamare:

`/[Controller]/[ActionName]/[Parameters]`

La prima parte dell'URL determina la classe controller da eseguire. Quindi */HelloWorld* esegue il mapping alla classe `HelloWorldController`. La seconda parte dell'URL determina il metodo di azione sulla classe da eseguire. Pertanto */HelloWorld/index* provocherebbe l'esecuzione del metodo `Index` della classe `HelloWorldController`. Si noti che è stato necessario passare a */HelloWorld* e per impostazione predefinita è stato usato il metodo `Index`. Questo perché un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato uno in modo esplicito.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il metodo `Welcome` viene eseguito e restituisce la stringa &quot;questo è il metodo di azione iniziale...&quot;. Il mapping MVC predefinito è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`. Non è stata ancora usata la parte `[Parameters]` dell'URL.

![](adding-a-controller/_static/image5.png)

Modificare leggermente l'esempio in modo che sia possibile passare informazioni sui parametri dall'URL al controller (ad esempio, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Modificare il metodo `Welcome` per includere due parametri, come illustrato di seguito. Si noti che il codice usa C# la funzionalità facoltativa-parametro per indicare che il parametro `numTimes` deve essere impostato su 1 se non viene passato alcun valore per il parametro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Eseguire l'applicazione e passare all'URL di esempio (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. È possibile provare diversi valori per `name` e `numtimes` nell'URL. Il [sistema di associazione di modelli MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) esegue automaticamente il mapping dei parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.

![](adding-a-controller/_static/image6.png)

In entrambi questi esempi il controller ha eseguito la &quot;VC&quot; parte di MVC, ovvero la visualizzazione e il controller funzionano. Il controller restituisce direttamente l'HTML. In genere, non si vuole che i controller restituiscano direttamente il codice HTML, dal momento che diventa molto complesso da codificare. Si userà in genere un file modello di vista separato per facilitare la generazione della risposta HTML. Esaminiamo ora come possiamo eseguire questa operazione.

> [!div class="step-by-step"]
> [Precedente](intro-to-aspnet-mvc-4.md)
> [Successivo](adding-a-view.md)
