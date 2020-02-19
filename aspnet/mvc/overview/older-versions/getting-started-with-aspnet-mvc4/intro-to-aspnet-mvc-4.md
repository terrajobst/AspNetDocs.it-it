---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Introduzione a ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Una versione aggiornata se questa esercitazione è disponibile qui usando Visual Studio 2013. La nuova esercitazione USA ASP.NET MVC 5, che fornisce molti miglioramenti rispetto a t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 51709a9c6ddb39b8fcd1cd94cd08d530a595825a
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455542"
---
# <a name="intro-to-aspnet-mvc-4"></a>Introduzione ad ASP.NET MVC 4

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Una versione aggiornata se questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). La nuova esercitazione USA ASP.NET MVC 5, che fornisce molti miglioramenti rispetto a questa esercitazione.
>
> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web ASP.NET MVC 4 utilizzando Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual Web Developer 2010 Express Service Pack 1. È consigliabile usare Visual Studio 2012, per completare l'esercitazione non è necessario installare alcun elemento. Se si usa Visual Studio 2010, è necessario installare i componenti seguenti. È possibile installarli tutti facendo clic sui collegamenti seguenti:
>
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Programma di installazione di WPI per ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare il programma di [installazione di WPI per ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) e il: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
>
> Per accompagnare questo argomento, C# è disponibile un progetto Visual Web Developer con codice sorgente. [Scaricare la C# versione](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
>
> Nell'esercitazione viene eseguita l'applicazione in Visual Studio. È anche possibile rendere disponibile l'applicazione su Internet distribuendo tale applicazione a un provider di hosting. Microsoft offre hosting web gratuito per un massimo di 10 siti Web in un [account di valutazione gratuito di Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Per informazioni su come distribuire un progetto Web di Visual Studio in un sito Web di Microsoft Azure, vedere [creare e distribuire un sito web ASP.NET e un database SQL con Visual Studio](https://docs.microsoft.com/dotnet/azure/). In questa esercitazione viene inoltre illustrato come utilizzare Migrazioni Code First di Entity Framework per distribuire il database SQL Server nel database SQL di Windows Azure (in precedenza SQL Azure).
>
> Questa esercitazione è stata scritta da Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).

## <a name="what-youll-build"></a>Scopo dell'esercitazione

> [!NOTE]
> Una versione aggiornata se questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). La nuova esercitazione USA ASP.NET MVC 5, che fornisce molti miglioramenti rispetto a questa esercitazione.

Verrà implementata una semplice applicazione per la visualizzazione di filmati che supporta la creazione, la modifica, la ricerca e l'elenco di filmati da un database. Di seguito sono riportate due schermate dell'applicazione da compilare. Include una pagina in cui viene visualizzato un elenco di filmati da un database:

![](intro-to-aspnet-mvc-4/_static/image1.png)

L'applicazione consente inoltre di aggiungere, modificare ed eliminare filmati, nonché di visualizzare i dettagli relativi a singoli utenti. Tutti gli scenari di immissione dei dati includono la convalida per garantire la correttezza dei dati archiviati nel database.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Guida introduttiva

Per iniziare, eseguire Visual Studio Express 2012 o Visual Web Developer 2010 Express. La maggior parte degli screenshot di questa serie usa Visual Studio Express 2012, ma è possibile completare questa esercitazione con Visual Studio 2010/SP1, Visual Studio 2012, Visual Studio Express 2012 o Visual Web Developer 2010 Express. Nella pagina **iniziale** selezionare **nuovo progetto** .

Visual Studio è un IDE o Integrated Development Environment. Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Studio è disponibile una barra degli strumenti nella parte superiore che mostra le varie opzioni disponibili. È anche disponibile un menu che fornisce un altro modo per eseguire attività nell'IDE. Ad esempio, invece di selezionare **nuovo progetto** dalla pagina **iniziale** , è possibile usare il menu e selezionare **file** &gt; **nuovo progetto**.

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Creazione della prima applicazione

È possibile creare applicazioni utilizzando Visual Basic o Visual C# come linguaggio di programmazione. Selezionare oggetto C# visivo a sinistra e quindi selezionare **ASP.NET MVC 4 Web Application**. Assegnare al progetto il nome &quot;MvcMovie&quot; e quindi fare clic su **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

Nella finestra di dialogo **nuovo progetto MVC 4 ASP.NET** selezionare **applicazione Internet**. Lasciare **Razor** come motore di visualizzazione predefinito.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Fare clic su **OK**. Visual Studio ha usato un modello predefinito per il progetto MVC ASP.NET appena creato ed è quindi disponibile un'applicazione funzionante senza eseguire alcuna operazione. Si tratta di una semplice &quot;Hello World!&quot; progetto ed è un posto ideale per avviare l'applicazione.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Scegliere **Avvia debug** dal menu **Debug**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Si noti che il tasto di scelta rapida per avviare il debug è F5.

F5 fa in modo che Visual Studio avvii IIS Express ed esegua l'applicazione Web. Visual Studio avvia quindi un browser e apre la home page dell'applicazione. Si noti che la barra degli indirizzi del browser indica `localhost` e non un elemento come `example.com`. Questo perché `localhost` fa sempre riferimento al computer locale, che in questo caso esegue l'applicazione appena creata. Quando si esegue un progetto Web in Visual Studio, viene utilizzata una porta casuale per il server Web. Nell'immagine seguente il numero di porta è 41788. Quando si esegue l'applicazione, è probabile che venga visualizzato un numero di porta diverso.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Per impostazione predefinita, questo modello predefinito fornisce Home, contatti e informazioni sulle pagine. Fornisce inoltre il supporto per la registrazione e l'accesso e i collegamenti a Facebook e Twitter. Il passaggio successivo consiste nel modificare il funzionamento di questa applicazione e apprendere un po' di ASP.NET MVC. Chiudere il browser e modificare il codice.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
