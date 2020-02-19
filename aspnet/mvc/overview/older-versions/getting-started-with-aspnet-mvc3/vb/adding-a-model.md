---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Aggiunta di un modello (VB) | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457245"
---
# <a name="adding-a-model-vb"></a>Aggiunta di un modello (VB)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, è disponibile un progetto Visual Web Developer con codice sorgente VB.NET. [Scaricare la versione di VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce C#, passare alla [ C# versione](../cs/adding-a-model.md) di questa esercitazione.

## <a name="adding-a-model"></a>Aggiunta di un modello

In questa sezione verranno aggiunte alcune classi per la gestione dei film in un database. Queste classi saranno la parte "modello" dell'applicazione MVC ASP.NET.

Si utilizzerà una tecnologia di accesso ai dati .NET Framework nota come Entity Framework per definire e utilizzare queste classi di modelli. Il Entity Framework (spesso definito EF) supporta un paradigma di sviluppo denominato *Code First*. Code First consente di creare oggetti modello scrivendo classi semplici. Questi sono anche noti come classi POCO, da "Plain-Old CLR Objects". È quindi possibile fare in modo che il database venga creato immediatamente dalle classi, che consente un flusso di lavoro di sviluppo molto pulito e rapido.

## <a name="adding-model-classes"></a>Aggiunta di classi di modelli

In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi selezionare **classe**.

![](adding-a-model/_static/image1.png)

Assegnare alla classe il nome "Movie".

Aggiungere le cinque proprietà seguenti alla classe `Movie`:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Si userà la classe `Movie` per rappresentare i film in un database. Ogni istanza di un oggetto `Movie` corrisponderà a una riga all'interno di una tabella di database e ogni proprietà della classe `Movie` eseguirà il mapping a una colonna della tabella.

Nello stesso file aggiungere la classe `MovieDBContext` seguente:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

La classe `MovieDBContext` rappresenta il contesto del database di Entity Framework Movie, che gestisce il recupero, l'archiviazione e l'aggiornamento delle istanze della classe `Movie` in un database. Il `MovieDBContext` deriva dalla classe di base `DbContext` fornita dal Entity Framework. Per ulteriori informazioni su `DbContext` e `DbSet`, vedere [miglioramenti della produttività per il Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Per poter fare riferimento `DbContext` e `DbSet`, è necessario aggiungere l'istruzione `imports` seguente all'inizio del file:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Il file completo *Movie. vb* è riportato di seguito.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Creazione di una stringa di connessione e utilizzo di SQL Server Compact

La classe `MovieDBContext` creata gestisce l'attività di connessione al database e di mapping `Movie` oggetti ai record del database. Una domanda che è possibile porre, tuttavia, è come specificare il database a cui si connetterà. A tale scopo, aggiungere le informazioni di connessione nel file *Web. config* dell'applicazione.

Aprire il file *Web. config* radice dell'applicazione. (Non il file *Web. config* nella cartella *views* ). L'immagine seguente mostra entrambi i file *Web. config* ; Aprire il file *Web. config* in un cerchio in rosso.

![](adding-a-model/_static/image2.png)

Aggiungere la stringa di connessione seguente all'elemento `<connectionStrings>` nel file *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Nell'esempio seguente viene illustrata una parte del file *Web. config* con la nuova stringa di connessione aggiunta:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Questa piccola quantità di codice e XML è tutto ciò che è necessario scrivere per rappresentare e archiviare i dati dei film in un database.

Successivamente, verrà compilata una nuova classe di `MoviesController` che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuovi elenchi di film.

> [!div class="step-by-step"]
> [Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)
