---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introduzione a ASP.NET MVC | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581585"
---
# <a name="intro-to-aspnet-mvc"></a>Introduzione ad ASP.NET MVC

di [Scott hanseln](https://github.com/shanselman)

> > [!NOTE]
> > Una versione aggiornata se questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). La nuova esercitazione USA ASP.NET MVC 5, che fornisce molti miglioramenti rispetto a questa esercitazione.
>
>
> Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Verrà creata una semplice applicazione Web che legge e scrive da un database. Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.

È ora necessario creare la prima applicazione Web MVC ASP.NET con [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Verrà creata un'applicazione di elenco di film che ci consentirà di creare ed elencare i film.

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Ecco due screenshot dell'applicazione che verrà compilata. Si disporrà di una semplice tabella di filmati con varie colonne.

[Elenco di film ![-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Ed è possibile creare un modulo per aggiungere film all'elenco.

[![creare un film-Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Acquisizione di competenze

Questa esercitazione illustra le nozioni di base per la creazione di un'applicazione Web MVC ASP.NET con Visual Studio. Si apprenderà come:

- Come creare un nuovo progetto MVC ASP.NET
- Come creare un nuovo database con SQL Server
- Come creare controller e visualizzazioni MVC ASP.NET
- Come recuperare e visualizzare i dati
- Come modificare i dati e abilitare la convalida dei dati
- Come aggiornare lo schema del database

## <a name="get-started"></a>Introduzione

Per iniziare, eseguire Visual Web Developer 2010 Express, che chiamerò "VWD" da ora in poi, e selezionare nuovo progetto dalla schermata Start.

Visual Web Developer è un IDE o un ambiente di sviluppo integrato. Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. Nella parte superiore sono disponibili una barra degli strumenti che mostra le varie opzioni disponibili, nonché il menu che è possibile usare per selezionare il file | Nuovo progetto.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Creazione della prima applicazione

È possibile creare applicazioni usando Visual Basic o Visual C#. Per il momento selezionare oggetto C# visivo a sinistra, quindi scegliere "ASP.NET MVC 2 Web Application". Denominare il progetto "Movies" e fare clic su OK.

[![nuovo progetto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Sul lato destro è presente la Esplora soluzioni che Mostra tutti i file e le cartelle dell'applicazione. La finestra grande al centro è la posizione in cui si modifica il codice e si dedica la maggior parte del tempo. Visual Studio ha usato un modello predefinito per il progetto MVC ASP.NET appena creato ed è quindi disponibile un'applicazione funzionante senza eseguire alcuna operazione. Si tratta di una semplice "Hello World! progetto ed è un punto di partenza ideale per l'applicazione.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Selezionare il pulsante "Riproduci" sulla barra degli strumenti.

![Avvia debug](getting-started-with-mvc-part1/_static/image11.png)

Si tratta di una freccia verde che punta a destra che compilerà il programma e avvierà l'applicazione in un Web browser.

*Nota: è possibile premere F5 sulla tastiera oppure selezionare debug-&gt;avviare il debug dal menu "debug".*

In questo modo, Visual Web Developer avvierà un server Web di sviluppo ed eseguirà l'applicazione Web (per abilitare questa operazione non è necessaria alcuna configurazione o procedura manuale). Viene quindi avviato un browser e configurato per l'esplorazione della Home page dell'applicazione. Si noti che la barra degli indirizzi del browser indica "localhost" e non un elemento come example.com. Questo perché localhost fa sempre riferimento al computer locale, che in questo caso esegue l'applicazione appena compilata.

[Home page di ![](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Questo modello predefinito fornisce due pagine da visitare e una pagina di accesso di base. Modificare il funzionamento di questa applicazione e apprendere un po' di ASP.NET MVC nel processo. Chiudere il browser e modificare il codice.

> [!div class="step-by-step"]
> [avanti](getting-started-with-mvc-part2.md)
