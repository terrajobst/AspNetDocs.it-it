---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Aggiunta di un modelloC#() | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a5f494eaa05bcfcd9d49873db728d71c1fd332c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540866"
---
# <a name="adding-a-model-c"></a>Aggiunta di un modello (C#)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, C# è disponibile un progetto Visual Web Developer con codice sorgente. [Scaricare la C# versione](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare alla [versione Visual Basic](../vb/adding-a-model.md) di questa esercitazione.

## <a name="adding-a-model"></a>Aggiunta di un modello

In questa sezione verranno aggiunte alcune classi per la gestione dei film in un database. Queste classi saranno la parte "modello" dell'applicazione MVC ASP.NET.

Si utilizzerà una tecnologia di accesso ai dati .NET Framework nota come Entity Framework per definire e utilizzare queste classi di modelli. Il Entity Framework (spesso definito EF) supporta un paradigma di sviluppo denominato *Code First*. Code First consente di creare oggetti modello scrivendo classi semplici. Questi sono anche noti come classi POCO, da "Plain-Old CLR Objects". È quindi possibile fare in modo che il database venga creato immediatamente dalle classi, che consente un flusso di lavoro di sviluppo molto pulito e rapido.

## <a name="adding-model-classes"></a>Aggiunta di classi di modelli

In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi selezionare **classe**.

![](adding-a-model/_static/image1.png)

Assegnare alla *classe* il nome "Movie".

[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)

Aggiungere le cinque proprietà seguenti alla classe `Movie`:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Si userà la classe `Movie` per rappresentare i film in un database. Ogni istanza di un oggetto `Movie` corrisponderà a una riga all'interno di una tabella di database e ogni proprietà della classe `Movie` eseguirà il mapping a una colonna della tabella.

Nello stesso file aggiungere la classe `MovieDBContext` seguente:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

La classe `MovieDBContext` rappresenta il contesto del database di Entity Framework Movie, che gestisce il recupero, l'archiviazione e l'aggiornamento delle istanze della classe `Movie` in un database. Il `MovieDBContext` deriva dalla classe di base `DbContext` fornita dal Entity Framework. Per ulteriori informazioni su `DbContext` e `DbSet`, vedere [miglioramenti della produttività per il Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Per poter fare riferimento `DbContext` e `DbSet`, è necessario aggiungere l'istruzione `using` seguente all'inizio del file:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Di seguito è riportato il file *Movie.cs* completo.

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Creazione di una stringa di connessione e utilizzo di SQL Server Compact

La classe `MovieDBContext` creata gestisce l'attività di connessione al database e di mapping `Movie` oggetti ai record del database. Una domanda che è possibile porre, tuttavia, è come specificare il database a cui si connetterà. A tale scopo, aggiungere le informazioni di connessione nel file *Web. config* dell'applicazione.

Aprire il file *Web. config* radice dell'applicazione. (Non il file *Web. config* nella cartella *views* ). L'immagine seguente mostra entrambi i file *Web. config* ; Aprire il file *Web. config* in un cerchio in rosso.

![](adding-a-model/_static/image4.png)

Aggiungere la stringa di connessione seguente all'elemento `<connectionStrings>` nel file *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Nell'esempio seguente viene illustrata una parte del file *Web. config* con la nuova stringa di connessione aggiunta:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Questa piccola quantità di codice e XML è tutto ciò che è necessario scrivere per rappresentare e archiviare i dati dei film in un database.

Successivamente, verrà compilata una nuova classe di `MoviesController` che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuovi elenchi di film.

> [!div class="step-by-step"]
> [Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)
