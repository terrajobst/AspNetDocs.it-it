---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: 'Esercitazione: Introduzione a EF Database First con MVC 5'
description: Questa esercitazione illustra come iniziare con un database esistente e come creare rapidamente un'applicazione Web che consente agli utenti di interagire con i dati.
author: Rick-Anderson
ms.author: riande
ms.date: 01/15/2019
ms.topic: tutorial
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: a760767839a834a9c7e9fe358a3fd806a833261f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583524"
---
# <a name="tutorial-get-started-with-ef-database-first-using-mvc-5"></a>Esercitazione: Introduzione a EF Database First con MVC 5

Usando l'impalcatura MVC, Entity Framework e ASP.NET, è possibile creare un'applicazione Web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare automaticamente il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database. Nell'ultima parte della serie si apprenderà come aggiungere annotazioni di dati al modello di dati per specificare i requisiti di convalida e la formattazione della visualizzazione. Al termine, è possibile passare a un articolo di Azure per informazioni su come distribuire un'app .NET e un database SQL in app Azure servizio.

Questa esercitazione illustra come iniziare con un database esistente e come creare rapidamente un'applicazione Web che consente agli utenti di interagire con i dati. Usa la Entity Framework 6 e MVC 5 per compilare l'applicazione Web. La funzionalità di ponteggi ASP.NET consente di generare automaticamente il codice per la visualizzazione, l'aggiornamento, la creazione e l'eliminazione di dati. Usando gli strumenti di pubblicazione in Visual Studio, è possibile distribuire facilmente il sito e il database in Azure.

Questa parte della serie è incentrata sulla creazione di un database e sulla relativa compilazione con i dati.

Questa serie è stata scritta con i contributi di Tom Dykstra e Rick Anderson. È stato migliorato in base ai commenti e suggerimenti degli utenti nella sezione dei commenti.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Configurare il database

## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="set-up-the-database"></a>Configurare il database

Per simulare l'ambiente in cui è presente un database esistente, è necessario innanzitutto creare un database con alcuni dati precompilati, quindi creare l'applicazione Web che si connette al database.

Questa esercitazione è stata sviluppata usando il database locale con Visual Studio 2017. È possibile utilizzare un server di database esistente anziché il database locale, ma a seconda della versione di Visual Studio e del tipo di database, tutti gli strumenti dati in Visual Studio potrebbero non essere supportati. Se gli strumenti non sono disponibili per il database, potrebbe essere necessario eseguire alcuni dei passaggi specifici del database all'interno di Management Suite per il database.

Se si verifica un problema con gli strumenti di database nella versione di Visual Studio in uso, assicurarsi di aver installato la versione più recente degli strumenti di database. Per informazioni sull'aggiornamento o l'installazione degli strumenti di database, vedere [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Avviare Visual Studio e creare un **progetto di Database SQL Server**. Denominare il progetto **ContosoUniversityData**.

![Creazione progetto di database](setting-up-database/_static/image1.png)

A questo punto è disponibile un progetto di database vuoto. Per assicurarsi che sia possibile distribuire il database in Azure, impostare il database SQL di Azure come piattaforma di destinazione per il progetto. L'impostazione della piattaforma di destinazione non comporta effettivamente la distribuzione del database. significa solo che il progetto di database verificherà che la progettazione del database sia compatibile con la piattaforma di destinazione. Per impostare la piattaforma di destinazione, aprire le **Proprietà** del progetto e selezionare **database SQL di Microsoft Azure** per la piattaforma di destinazione.

Per creare le tabelle necessarie per questa esercitazione, è possibile aggiungere script SQL che definiscono le tabelle. Fare clic con il pulsante destro del mouse sul progetto e aggiungere un nuovo elemento. Selezionare le **tabelle e le viste** > **tabella** e denominarla *Student*.

Nel file di tabella sostituire il comando T-SQL con il codice seguente per creare la tabella.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Si noti che la finestra di progettazione viene sincronizzata automaticamente con il codice. È possibile utilizzare il codice o la finestra di progettazione.

![Mostra codice e progettazione](setting-up-database/_static/image5.png)

Aggiungere un'altra tabella. Questa volta denominare il corso e usare il comando T-SQL seguente.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Per creare una tabella denominata registrazione, ripetere ancora una volta.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

È possibile popolare il database con i dati tramite uno script che viene eseguito dopo la distribuzione del database. Aggiungere uno script di post-distribuzione al progetto. Fare clic con il pulsante destro del mouse sul progetto e aggiungere un nuovo elemento. Selezionare script **utente** > **script post-distribuzione**. È possibile utilizzare il nome predefinito.

Aggiungere il seguente codice T-SQL allo script post-distribuzione. Questo script aggiunge semplicemente i dati al database quando non viene trovato alcun record corrispondente. Non sovrascrive né elimina i dati che potrebbero essere stati immessi nel database.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

È importante notare che lo script di post-distribuzione viene eseguito ogni volta che si distribuisce il progetto di database. Pertanto, è necessario considerare attentamente i requisiti durante la scrittura di questo script. In alcuni casi, è possibile che si desideri ripartire da un set di dati noto ogni volta che viene distribuito il progetto. In altri casi, è possibile che non si desideri modificare i dati esistenti in alcun modo. In base ai requisiti, è possibile decidere se è necessario uno script di post-distribuzione o ciò che è necessario includere nello script. Per ulteriori informazioni sul popolamento del database con uno script di post-distribuzione, vedere [inclusione di dati in un progetto di database SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Sono ora disponibili 4 file script SQL, ma nessuna tabella effettiva. Si è pronti per distribuire il progetto di database nel database locale. In Visual Studio fare clic sul pulsante Start (o F5) per compilare e distribuire il progetto di database. Controllare la scheda **output** per verificare che la compilazione e la distribuzione siano state completate correttamente.

Per verificare che il nuovo database sia stato creato, aprire **Esplora oggetti di SQL Server** e cercare il nome del progetto nel server di database locale corretto (in questo caso (local DB **) \ProjectsV13**).

Per verificare che le tabelle siano popolate con i dati, fare clic con il pulsante destro del mouse su una tabella e selezionare **Visualizza dati**.

![Mostra dati tabella](setting-up-database/_static/image9.png)

Viene visualizzata una visualizzazione modificabile dei dati della tabella. Se, ad esempio, si selezionano **tabelle** > **dbo. Course** > **visualizzare i dati**, viene visualizzata una tabella con tre colonne (**corso**, **titolo**e **crediti**) e quattro righe.

## <a name="additional-resources"></a>Risorse aggiuntive

Per un esempio introduttivo di sviluppo Code First, vedere [Introduzione con ASP.NET MVC 5](../introduction/getting-started.md). Per un esempio più avanzato, vedere la pagina relativa alla [creazione di un modello di dati Entity Framework per un'App ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Per indicazioni sulla scelta dell'approccio Entity Framework da usare, vedere [Entity Framework approcci di sviluppo](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Configurare il database

Passare all'esercitazione successiva per apprendere come creare l'applicazione Web e i modelli di dati.
> [!div class="nextstepaction"]
> [Creare l'applicazione Web e i modelli di dati](creating-the-web-application.md)
