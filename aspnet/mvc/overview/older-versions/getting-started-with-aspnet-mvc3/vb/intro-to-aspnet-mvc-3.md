---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Introduzione a ASP.NET MVC 3 (VB) | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 24f7de303ef7f5a457bd509ecc6bd0e3be7e3d9d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456325"
---
# <a name="intro-to-aspnet-mvc-3-vb"></a>Introduzione ad ASP.NET MVC 3 (VB)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, è disponibile un progetto Visual Web Developer con codice sorgente VB.NET. [Scaricare la versione di VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce C#, passare alla [ C# versione](../cs/intro-to-aspnet-mvc-3.md) di questa esercitazione.

In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:

- [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)

Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Per accompagnare questo argomento, è disponibile un progetto Visual Web Developer con codice sorgente VB. [Scaricare la versione VB qui](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Se si preferisce CSharp, passare alla [versione CSharp](../cs/intro-to-aspnet-mvc-3.md) di questa esercitazione.

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Verrà implementata una semplice applicazione per la visualizzazione di filmati che supporta la creazione, la modifica e l'inserimento di filmati da un database. Di seguito sono riportate due schermate dell'applicazione da compilare. Include una pagina in cui viene visualizzato un elenco di filmati da un database:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

L'applicazione consente inoltre di aggiungere, modificare ed eliminare filmati, nonché di visualizzare i dettagli relativi a singoli utenti. Tutti gli scenari di immissione dei dati includono la convalida per garantire la correttezza dei dati archiviati nel database.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Acquisizione di competenze

In questa esercitazione si apprenderà:

- Come creare un nuovo progetto MVC ASP.NET
- Come creare un nuovo database usando Entity Framework Code-First
- Come creare controller e visualizzazioni MVC ASP.NET
- Come recuperare e visualizzare i dati
- Come modificare i dati e abilitare la convalida dei dati

## <a name="getting-started"></a>Guida introduttiva

Per iniziare, eseguire Visual Web Developer 2010 Express ("VWD" per brevità) e selezionare **nuovo progetto** nella pagina **iniziale** .

Visual Web Developer è un IDE o Integrated Development Environment. Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Web Developer è presente una barra degli strumenti nella parte superiore che mostra le varie opzioni disponibili. È anche disponibile un menu che fornisce un altro modo per eseguire attività nell'IDE. Ad esempio, invece di selezionare **nuovo progetto** dalla pagina **iniziale** , è possibile usare il menu e selezionare **file** &gt; **nuovo progetto**.

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Creazione della prima applicazione

È possibile creare applicazioni usando la scelta di Visual Basic o visuale C# come linguaggio di programmazione. Per questa esercitazione selezionare Visual Basic a sinistra, quindi selezionare **applicazione Web MVC 3 ASP.NET**. Assegnare al progetto il nome "MvcMovie" e quindi fare clic su **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

Nella finestra di dialogo **nuovo progetto MVC 3 ASP.NET** selezionare **applicazione Internet**. Lasciare **Razor** come motore di visualizzazione predefinito.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Fare clic su **OK**. In Visual Web Developer è stato usato un modello predefinito per il progetto MVC ASP.NET appena creato, quindi è disponibile un'applicazione funzionante senza eseguire alcuna operazione. Si tratta di una semplice "Hello World!" il progetto è una soluzione ideale per avviare l'applicazione.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Scegliere **Avvia debug** dal menu **Debug**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Si noti che il tasto di scelta rapida per avviare il debug è F5.

F5 fa in modo che Visual Web Developer avvii un server Web di sviluppo ed esegua l'applicazione Web. VWD avvia quindi un browser e apre la home page dell'applicazione. Si noti che la barra degli indirizzi del browser indica `localhost` e non un elemento come `example.com`. Questo perché `localhost` fa sempre riferimento al computer locale, che in questo caso esegue l'applicazione appena creata. Quando VWD esegue un progetto Web, per il progetto viene utilizzata una porta casuale. Nell'immagine seguente il numero di porta casuale è 43246. Il progetto utilizzerà probabilmente un numero di porta diverso.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Questo modello predefinito fornisce due pagine da visitare e una pagina di accesso di base. Modificare il funzionamento di questa applicazione e apprendere un po' di ASP.NET MVC nel processo. Chiudere il browser e modificare il codice.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
