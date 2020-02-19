---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introduzione a ASP.NET MVC 3 (C#) | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: e71275c93558c0b6ca087a145786e8c846b69721
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457532"
---
# <a name="intro-to-aspnet-mvc-3-c"></a>Introduzione ad ASP.NET MVC 3 (C#)

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

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Verrà implementata una semplice applicazione per la visualizzazione di filmati che supporta la creazione, la modifica e l'inserimento di filmati da un database. Di seguito sono riportate due schermate dell'applicazione da compilare. Include una pagina in cui viene visualizzato un elenco di filmati da un database:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

L'applicazione consente inoltre di aggiungere, modificare ed eliminare filmati, nonché di visualizzare i dettagli relativi a singoli utenti. Tutti gli scenari di immissione dei dati includono la convalida per garantire la correttezza dei dati archiviati nel database.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Acquisizione di competenze

In questa esercitazione si apprenderà:

- Come creare un nuovo progetto MVC ASP.NET.
- Come creare controller e visualizzazioni MVC ASP.NET.
- Come creare un nuovo database usando il paradigma Entity Framework Code First.
- Come recuperare e visualizzare i dati.
- Come modificare i dati e abilitare la convalida dei dati.

## <a name="getting-started"></a>Guida introduttiva

Per iniziare, eseguire Visual Web Developer 2010 Express ("Visual Web Developer" per brevità) e selezionare **nuovo progetto** nella pagina **iniziale** .

Visual Web Developer è un IDE o Integrated Development Environment. Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Web Developer è presente una barra degli strumenti nella parte superiore che mostra le varie opzioni disponibili. È anche disponibile un menu che fornisce un altro modo per eseguire attività nell'IDE. Ad esempio, invece di selezionare **nuovo progetto** dalla pagina **iniziale** , è possibile usare il menu e selezionare **file** &gt; **nuovo progetto**.

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Creazione della prima applicazione

È possibile creare applicazioni utilizzando Visual Basic o Visual C# come linguaggio di programmazione. Selezionare oggetto C# visivo a sinistra e quindi selezionare **ASP.NET MVC 3 Web Application**. Assegnare al progetto il nome "MvcMovie" e quindi fare clic su **OK**. Se si preferisce Visual Basic, passare alla [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.

![](intro-to-aspnet-mvc-3/_static/image5.png)

Nella finestra di dialogo **nuovo progetto MVC 3 ASP.NET** selezionare **applicazione Internet**. Selezionare **Usa markup HTML5** e lascia **Razor** come motore di visualizzazione predefinito.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Fare clic su **OK**. In Visual Web Developer è stato usato un modello predefinito per il progetto MVC ASP.NET appena creato, quindi è disponibile un'applicazione funzionante senza eseguire alcuna operazione. Si tratta di una semplice "Hello World!" il progetto è una soluzione ideale per avviare l'applicazione.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

Scegliere **Avvia debug** dal menu **Debug**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Si noti che il tasto di scelta rapida per avviare il debug è F5.

F5 fa in modo che Visual Web Developer avvii un server Web di sviluppo ed esegua l'applicazione Web. Visual Web Developer avvia quindi un browser e apre la home page dell'applicazione. Si noti che la barra degli indirizzi del browser indica `localhost` e non un elemento come `example.com`. Questo perché `localhost` fa sempre riferimento al computer locale, che in questo caso esegue l'applicazione appena creata. Quando Visual Web Developer esegue un progetto Web, viene utilizzata una porta casuale per il server Web. Nell'immagine seguente il numero di porta casuale è 43246. Quando si esegue l'applicazione, è probabile che venga visualizzato un numero di porta diverso.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Il modello predefinito fornisce due pagine da visitare e una pagina di accesso di base. Il passaggio successivo consiste nel modificare il funzionamento di questa applicazione e apprendere un po' di ASP.NET MVC nel processo. Chiudere il browser e modificare il codice.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
