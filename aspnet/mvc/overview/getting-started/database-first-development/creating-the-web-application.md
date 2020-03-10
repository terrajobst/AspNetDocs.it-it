---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: "Esercitazione: creare l'applicazione Web e i modelli di dati per Database First EF con ASP.NET MVC"
description: Questa esercitazione è incentrata sulla creazione dell'applicazione Web e sulla generazione di modelli di dati basati sulle tabelle di database.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616270"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Esercitazione: creare l'applicazione Web e i modelli di dati per Database First EF con ASP.NET MVC

 Usando l'impalcatura MVC, Entity Framework e ASP.NET, è possibile creare un'applicazione Web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare automaticamente il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database.

Questa esercitazione è incentrata sulla creazione dell'applicazione Web e sulla generazione di modelli di dati basati sulle tabelle di database.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare un'app Web ASP.NET
> * Generare i modelli

## <a name="prerequisites"></a>Prerequisiti

* [Introduzione a Entity Framework 6 Database First con MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>Creare un'app Web ASP.NET

In una nuova soluzione o nella stessa soluzione del progetto di database, creare un nuovo progetto in Visual Studio e selezionare il modello **applicazione Web ASP.NET** . Denominare il progetto **ContosoSite**.

![crea progetto](creating-the-web-application/_static/image1.png)

Fare clic su **OK**.

Nella finestra nuovo progetto ASP.NET selezionare il modello **MVC** . Per il momento, è possibile cancellare l' **host nell'opzione cloud** perché l'applicazione verrà distribuita nel cloud in un secondo momento. Fare clic su **OK** per creare l'applicazione.

Il progetto viene creato con i file e le cartelle predefiniti.

In questa esercitazione si userà Entity Framework 6. È possibile controllare la versione di Entity Framework nel progetto tramite la finestra Gestisci pacchetti NuGet. Se necessario, aggiornare la versione di Entity Framework.

![Mostra versione](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Generare i modelli

A questo punto si creeranno Entity Framework modelli dalle tabelle di database. Questi modelli sono classi che verranno usate per lavorare con i dati. Ogni modello rispecchia una tabella nel database e contiene proprietà che corrispondono alle colonne della tabella.

Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi** e **nuovo elemento**.

Nella finestra Aggiungi nuovo elemento selezionare i **dati** nel riquadro a sinistra e **ADO.NET Entity Data Model** dalle opzioni nel riquadro centrale. Denominare il nuovo file di modello **ContosoModel**.

Fare clic su **Aggiungi**.

Nella procedura guidata di Entity Data Model selezionare **EF Designer from database**.

Fare clic su **Avanti**.

Se nell'ambiente di sviluppo sono definite connessioni di database, è possibile che una di queste connessioni sia già selezionata. Tuttavia, si desidera creare una nuova connessione al database creato nella prima parte di questa esercitazione. Fare clic sul pulsante **nuova connessione** .

Nella Finestra Proprietà di connessione specificare il nome del server locale in cui è stato creato il database, in questo caso **(local DB) \ProjectsV13**). Dopo aver fornito il nome del server, selezionare il ContosoUniversityData dai database disponibili.

![impostare le proprietà di connessione](creating-the-web-application/_static/image8.png)

Fare clic su **OK**.

Verranno ora visualizzate le proprietà di connessione corrette. È possibile utilizzare il nome predefinito per la connessione nel file Web. config.

Fare clic su **Avanti**.

Selezionare la versione più recente di Entity Framework.

Fare clic su **Avanti**.

Selezionare le **tabelle** per generare modelli per tutte e tre le tabelle.

Scegliere **Fine**.

Se viene visualizzato un avviso di sicurezza, selezionare **OK** per continuare a eseguire il modello.

I modelli vengono generati dalle tabelle di database e viene visualizzato un diagramma che mostra le proprietà e le relazioni tra le tabelle.

![diagramma del modello](creating-the-web-application/_static/image11.png)

La cartella Models include ora molti nuovi file correlati ai modelli generati dal database.

Il file **ContosoModel.Context.cs** contiene una classe che deriva dalla classe **DbContext** e fornisce una proprietà per ogni classe del modello che corrisponde a una tabella di database. I file **Course.cs**, **Enrollment.cs**e **Student.cs** contengono le classi del modello che rappresentano le tabelle dei database. Quando si utilizzano le impalcature, si utilizzeranno sia la classe Context sia le classi del modello.

Prima di procedere con questa esercitazione, compilare il progetto. Nella sezione successiva verrà generato codice basato sui modelli di dati, ma questa sezione non funzionerà se il progetto non è stato compilato.

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creazione di un'app Web ASP.NET
> * Generazione dei modelli

Passare all'esercitazione successiva per apprendere come creare codice di generazione basato sui modelli di dati.
> [!div class="nextstepaction"]
> [Generazione di visualizzazioni](generating-views.md)
