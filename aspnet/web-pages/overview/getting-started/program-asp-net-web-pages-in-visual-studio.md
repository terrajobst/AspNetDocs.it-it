---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmazione Pagine Web ASP.NET (Razor) con Visual Studio | Microsoft Docs
author: Rick-Anderson
description: In questa appendice viene illustrato come è possibile utilizzare Visual Studio 2010 o Visual Web Developer 2010 Express per programmare Pagine Web ASP.NET con il sintassi Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 1a76098779d05912bf7bdf2de5fdce024770752c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633511"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programmazione Pagine Web ASP.NET (Razor) con Visual Studio

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come è possibile usare Visual Studio o Visual Web Developer Express per i siti Web di programma Pagine Web ASP.NET (Razor).
>
> Contenuto dell'esercitazione
>
> - Cosa è necessario installare (se necessario) per lavorare con Pagine Web ASP.NET nella versione di Visual Studio.
> - Come aggiungere il supporto per Pagine Web ASP.NET a Visual Web Developer 2010 Express.
> - Come usare le funzionalità di Visual Studio per lavorare con le pagine Razor di ASP.NET, tra cui IntelliSense e il debugger.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
>
> - Pagine Web ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>
>
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2, Visual Studio 2012, Visual Studio 2010 e WebMatrix 2.

È possibile programmare le pagine Web di ASP.NET con sintassi Razor usando WebMatrix o molti altri editor di codice. È anche possibile usare Microsoft Visual Studio che è un Integrated Development Environment completo (IDE) che fornisce un potente set di strumenti per la creazione di molti tipi di applicazioni (non solo siti Web). Per lavorare con ASP.NET Razor Pages, è possibile usare [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Due funzionalità particolarmente utili fornite da Visual Studio per la programmazione con le pagine Web di ASP.NET Razor sono:

- *IntelliSense*. La funzionalità IntelliSense incorporata in Visual Studio è più completa di IntelliSense in WebMatrix.
- *Debugger*. Il debugger consente di risolvere i problemi del codice arrestando un programma mentre è in esecuzione, esaminando le variabili ed eseguendo il codice riga per riga.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Uso di Visual Studio con versioni diverse di Pagine Web ASP.NET

Per sviluppare app Web ASP.NET in Visual Studio 2017, installare il carico di lavoro **sviluppo di ASP.NET e Web** .

Visual Studio 2012 e Visual Studio 2013 includono il supporto per Pagine Web ASP.NET. I pacchetti necessari per supportare Pagine Web ASP.NET vengono installati quando si installa Visual Studio.

Per impostazione predefinita, in Visual Studio 2010 non è incluso il supporto per Pagine Web ASP.NET. Per usare Pagine Web ASP.NET con Visual Studio 2010, è necessario installare il pacchetto MVC ASP.NET. Per ottenere Pagine Web ASP.NET 2, installare ASP.NET MVC 4.

Nella tabella seguente viene riepilogato il supporto per Pagine Web ASP.NET in versioni diverse di Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **Pagine Web ASP.NET 2** | Installare ASP.NET MVC 4 | Incluso | Incluso |
| **Pagine Web ASP.NET 3** |  | Eseguire l'aggiornamento a Pagine Web ASP.NET 3 tramite NuGet | Incluso |

Per lavorare con Visual Studio 2010, vedere [installazione del supporto per pagine Web ASP.NET in Visual studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Avvio di Visual Studio da WebMatrix

Se è stato avviato un progetto in WebMatrix e si vuole passare a Visual Studio, WebMatrix fornisce un pulsante per aprire facilmente il progetto in Visual Studio. Per abilitare questo pulsante è necessario che Visual Studio sia installato nel computer. La figura seguente mostra il pulsante in WebMatrix.

![avvia Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Quando si fa clic sul pulsante, il progetto viene aperto in Visual Studio. È possibile passare da WebMatrix a Visual Studio e viceversa senza problemi. Si riceverà una notifica se i file sono stati modificati nell'altro ambiente ed è necessario ricaricarli per ottenere le modifiche più recenti.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Creazione del sito Razor di ASP.NET in Visual Studio

Per creare un sito Web ASP.NET Razor in Visual Studio:

1. Aprire Visual Studio.
2. Scegliere **nuovo sito Web**dal menu **file** .

    ![Crea nuovo sito Web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. Nella finestra di dialogo **nuovo sito Web** selezionare la lingua da utilizzare (Visual C# o Visual Basic).
4. Selezionare il modello di **sito Web ASP.NET (Razor)** .

    ![sito Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Fare clic su **OK**.

Il nuovo progetto esiste e viene popolato con alcune pagine Web predefinite che consentono di iniziare.

### <a name="using-intellisense"></a>Using IntelliSense

Ora che è stato creato un sito, è possibile vedere come funziona IntelliSense in Visual Studio.

1. Nel sito Web appena creato aprire la pagina *default. cshtml* .
2. Dopo i tag `<h3>` nella pagina digitare `@ServerInfo.` (incluso il punto). Si noti che IntelliSense Visualizza i metodi disponibili per l'helper `ServerInfo` in un elenco a discesa.

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Selezionare il metodo `GetHtml` dall'elenco e quindi premere INVIO. IntelliSense compila automaticamente il metodo. Come con qualsiasi metodo in C#, è necessario aggiungere `()` caratteri dopo il metodo. Il codice completo per il metodo `GetHtml` è simile all'esempio seguente:

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Premere CTRL + F5 per eseguire la pagina. Questo è l'aspetto della pagina quando viene visualizzato in un browser:

    ![pagina predefinita nel browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Chiudere il browser.

### <a name="using-the-debugger"></a>Uso del debugger

1. Nella parte superiore della pagina *default. cshtml* , dopo la riga che inizia con `Page.Title`, aggiungere la riga di codice seguente:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Nel margine grigio dell'editor a sinistra del codice, fare clic su Avanti accanto alla nuova riga per aggiungere un punto di *interruzione*. Un punto di interruzione è un indicatore che indica al debugger di arrestare l'esecuzione del programma in quel momento, in modo da poter vedere cosa accade.

    ![Imposta punto di interruzione](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Rimuovere la chiamata al metodo `ServerInfo.GetHtml` e aggiungere una chiamata alla variabile `@myTime` al suo posto. Questa chiamata consente di visualizzare il valore dell'ora corrente restituito dalla nuova riga di codice.
4. Premere F5 per eseguire la pagina nel debugger. La pagina viene arrestata in corrispondenza del punto di interruzione impostato. La figura seguente mostra l'aspetto della pagina nell'editor con il punto di interruzione (in giallo).

    ![punto di interruzione debug](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Sulla barra degli strumenti Debug fare clic sul pulsante Esegui **istruzione** (o premere F11) per eseguire la riga di codice successiva. Ogni volta che si fa clic su questo pulsante, l'esecuzione viene spostata alla riga di codice successiva.

    ![Pulsante Esegui istruzione](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Esaminare il valore della variabile `myTime` tenendo premuto il puntatore del mouse o controllando i valori visualizzati nelle finestre **variabili locali** e **stack di chiamate** . Visual Studio Visualizza il valore della variabile.

    ![Mostra valore ora](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Quando si è terminato di esaminare la variabile ed eseguire il codice istruzione per istruzione, premere F5 per continuare a eseguire la pagina senza fermarsi a ogni riga. Al termine dell'esecuzione del codice, il browser Visualizza la pagina.

Per ulteriori informazioni sul debugger e su come eseguire il debug di codice in Visual Studio, vedere [procedura dettagliata: debug di pagine Web in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Uso di Razor nei progetti MVC ASP.NET con Visual Studio

Il sintassi Razor viene inoltre ampiamente utilizzato nei progetti MVC ASP.NET. MVC è un metodo potente basato su modelli per la creazione di siti Web dinamici. Se il sito di Pagine Web ASP.NET diventa difficile da gestire, è consigliabile provare a convertirlo in un'applicazione MVC ASP.NET. Per un esempio di creazione di un'applicazione MVC, vedere [Introduzione con ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Installazione del supporto per Pagine Web ASP.NET in Visual Studio 2010

In questa sezione viene illustrato come installare Visual Web Developer Express 2010 e gli strumenti di Pagine Web ASP.NET per Visual Studio.

1. Se non si dispone ancora dell'installazione guidata piattaforma Web, scaricarla dall'URL seguente:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Eseguire l'installazione guidata piattaforma Web.
3. Fare clic sulla scheda **prodotti** .

    ![Scheda prodotti WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Cercare **ASP.NET MVC 4** (per pagine Web ASP.NET 2) e quindi fare clic su **Aggiungi**. Questi prodotti includono Visual Studio Tools per la creazione di siti Web Razor ASP.NET.

    ![Opzioni di installazione di WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Fare clic su **Installa** per completare l'installazione.
