---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Aggiunta di un controller (VB) | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2e77f62a9796211b0e59a99c71bc532659b7cb92
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457402"
---
# <a name="adding-a-controller-vb"></a>Aggiunta di un controller (VB)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, è disponibile un progetto Visual Web Developer con codice sorgente VB.NET. [Scaricare la versione di VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce C#, passare alla [ C# versione](../cs/adding-a-controller.md) di questa esercitazione.

MVC è l'acronimo di *Model-View-Controller*. MVC è un modello per lo sviluppo di applicazioni in modo che ogni parte abbia una responsabilità separata:

- Modello: i dati per l'applicazione.
- Views: i file modello che l'applicazione userà per generare dinamicamente risposte HTML.
- Controller: classi che gestiscono le richieste URL in ingresso per l'applicazione, recuperano i dati del modello e quindi specificano i modelli di visualizzazione che eseguono il rendering di una risposta al client.

Verranno trattati tutti questi concetti in questa esercitazione e verrà illustrato come usarli per creare un'applicazione.

Per creare un nuovo controller, fare clic con il pulsante destro del mouse sulla cartella *Controllers* in **Esplora soluzioni** e quindi scegliere **Aggiungi controller**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Assegnare un nome al nuovo controller &quot;HelloWorldController&quot; e fare clic su **Aggiungi**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Si noti che in **Esplora soluzioni** a destra è stato creato un nuovo file denominato *HelloWorldController.cs* e che il file è aperto nell'IDE.

All'interno del nuovo blocco `public class HelloWorldController` creare due nuovi metodi simili al codice riportato di seguito. Verrà restituita una stringa di codice HTML direttamente dal controller come esempio.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Il controller è denominato `HelloWorldController` e il nuovo metodo è denominato `Index`. Eseguire l'applicazione (premere F5 o CTRL + F5). Al termine dell'avvio del browser, aggiungere &quot;HelloWorld&quot; al percorso nella barra degli indirizzi. (Sul mio computer è `http://localhost:43246/HelloWorld`) Il browser sarà simile alla schermata seguente. Nel metodo precedente, il codice ha restituito direttamente una stringa. Abbiamo comunicato al sistema di restituire solo codice HTML,

![](adding-a-controller/_static/image5.png)

ASP.NET MVC richiama diverse classi controller (e metodi di azione diversi al loro interno) a seconda dell'URL in ingresso. La logica di mapping predefinita usata da ASP.NET MVC usa un formato simile al seguente per controllare quale codice viene richiamato:

`/[Controller]/[ActionName]/[Parameters]`

La prima parte dell'URL determina la classe controller da eseguire. Quindi */HelloWorld* esegue il mapping alla classe `HelloWorldController`. La seconda parte dell'URL determina il metodo di azione sulla classe da eseguire. Pertanto */HelloWorld/index* provocherebbe l'esecuzione del metodo `Index` della classe `HelloWorldController`. Si noti che è stato necessario solo visitare */HelloWorld* e il metodo `Index` è stato usato per impostazione predefinita. Questo perché un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato uno in modo esplicito.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il metodo `Welcome` viene eseguito e restituisce la stringa &quot;questo è il metodo di azione iniziale...&quot;. Il mapping MVC predefinito è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller è `HelloWorld` e `Welcome` è il metodo. Non è ancora stato usato il `[Parameters]` parte dell'URL.

![](adding-a-controller/_static/image6.png)

Modificare leggermente l'esempio in modo che sia possibile passare le informazioni sui parametri dall'URL al controller (ad esempio, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Modificare il metodo `Welcome` per includere due parametri, come illustrato di seguito. Si noti che è stata usata la funzionalità di parametro VB facoltativo per indicare che il parametro `numTimes` deve essere impostato su 1 se non viene passato alcun valore per il parametro.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Eseguire l'applicazione e passare a `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** È possibile provare valori diversi per `name` e `numtimes`. Il sistema esegue automaticamente il mapping dei parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.

![](adding-a-controller/_static/image7.png)

In entrambi questi esempi il controller sta eseguendo la parte VC di MVC, ovvero la visualizzazione e il funzionamento del controller. Il controller restituisce direttamente l'HTML. In genere, non si vuole che i controller restituiscano direttamente HTML, perché questo diventa molto complesso per il codice. Si userà in genere un file modello di vista separato per facilitare la generazione della risposta HTML. Esaminiamo ora come possiamo eseguire questa operazione.

> [!div class="step-by-step"]
> [Precedente](intro-to-aspnet-mvc-3.md)
> [Successivo](adding-a-view.md)
