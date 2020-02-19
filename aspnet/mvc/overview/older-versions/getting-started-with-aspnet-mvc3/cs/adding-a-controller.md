---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Aggiunta di un controllerC#() | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 959116ff773f4ef466cda6b172e8321590b50e5b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457831"
---
# <a name="adding-a-controller-c"></a>Aggiunta di un controller (C#)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.
> 
> 
> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, C# è disponibile un progetto Visual Web Developer con codice sorgente. [Scaricare la C# versione](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare alla [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.

MVC è l'acronimo di *Model-View-Controller*. MVC è un modello per lo sviluppo di applicazioni ben progettate e facili da gestire. Le applicazioni basate su MVC contengono:

- Controller: classi che gestiscono le richieste in ingresso all'applicazione, recuperano i dati del modello e quindi specificano i modelli di visualizzazione che restituiscono una risposta al client.
- Modelli: classi che rappresentano i dati dell'applicazione e che utilizzano la logica di convalida per applicare le regole business per tali dati.
- Visualizzazioni: file modello usati dall'applicazione per generare dinamicamente risposte HTML.

Verranno trattati tutti questi concetti in questa serie di esercitazioni e verrà illustrato come usarli per creare un'applicazione.

Iniziamo creando una classe controller. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *controller* e quindi scegliere **Aggiungi controller**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Assegnare un nome al nuovo controller "HelloWorldController". Lasciare il modello predefinito come **controller vuoto** e fare clic su **Aggiungi**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Si noti che in **Esplora soluzioni** è stato creato un nuovo file denominato *HelloWorldController.cs*. Il file è aperto nell'IDE.

![](adding-a-controller/_static/image5.png)

All'interno del blocco `public class HelloWorldController` creare due metodi simili al codice riportato di seguito. Il controller restituirà una stringa di codice HTML come esempio.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Il controller è denominato `HelloWorldController` e il primo metodo precedente è denominato `Index`. Che verrà ora richiamato da un browser. Eseguire l'applicazione (premere F5 o CTRL + F5). Nel browser aggiungere "HelloWorld" al percorso nella barra degli indirizzi. Nell'illustrazione seguente, ad esempio, è `http://localhost:43246/HelloWorld.`) La pagina nel browser sarà simile alla schermata seguente. Nel metodo precedente, il codice ha restituito direttamente una stringa. Si è detto al sistema di restituire solo codice HTML,

![](adding-a-controller/_static/image6.png)

ASP.NET MVC richiama diverse classi controller (e metodi di azione diversi al loro interno) a seconda dell'URL in ingresso. La logica di mapping predefinita usata da ASP.NET MVC usa un formato simile al seguente per determinare il codice da richiamare:

`/[Controller]/[ActionName]/[Parameters]`

La prima parte dell'URL determina la classe controller da eseguire. Quindi */HelloWorld* esegue il mapping alla classe `HelloWorldController`. La seconda parte dell'URL determina il metodo di azione sulla classe da eseguire. Pertanto */HelloWorld/index* provocherebbe l'esecuzione del metodo `Index` della classe `HelloWorldController`. Si noti che è stato necessario passare a */HelloWorld* e per impostazione predefinita è stato usato il metodo `Index`. Questo perché un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato uno in modo esplicito.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il metodo `Welcome` viene eseguito e restituisce la stringa "This is the Welcome action method...". Il mapping MVC predefinito è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`. Non è stata ancora usata la parte `[Parameters]` dell'URL.

![](adding-a-controller/_static/image7.png)

Modificare leggermente l'esempio in modo che sia possibile passare informazioni sui parametri dall'URL al controller (ad esempio, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Modificare il metodo `Welcome` per includere due parametri, come illustrato di seguito. Si noti che il codice usa C# la funzionalità facoltativa-parametro per indicare che il parametro `numTimes` deve essere impostato su 1 se non viene passato alcun valore per il parametro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Eseguire l'applicazione e passare all'URL di esempio (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. È possibile provare diversi valori per `name` e `numtimes` nell'URL. Il sistema esegue automaticamente il mapping dei parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.

![](adding-a-controller/_static/image8.png)

In entrambi questi esempi il controller sta eseguendo la parte "VC" di MVC, ovvero la visualizzazione e il controller funzionano. Il controller restituisce direttamente l'HTML. In genere, non si vuole che i controller restituiscano direttamente il codice HTML, dal momento che diventa molto complesso da codificare. Si userà in genere un file modello di vista separato per facilitare la generazione della risposta HTML. Esaminiamo ora come possiamo eseguire questa operazione.

> [!div class="step-by-step"]
> [Precedente](intro-to-aspnet-mvc-3.md)
> [Successivo](adding-a-view.md)
