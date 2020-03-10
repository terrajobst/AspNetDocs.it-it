---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: "Esercitazione: modificare il database per Database First EF con l'app MVC ASP.NET"
description: Questa esercitazione è incentrata sulla creazione di un aggiornamento della struttura del database e sulla propagazione di tale modifica nell'intera applicazione Web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616263"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>Esercitazione: modificare il database per Database First EF con l'app MVC ASP.NET

Usando l'impalcatura MVC, Entity Framework e ASP.NET, è possibile creare un'applicazione Web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare automaticamente il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database.

Questa esercitazione è incentrata sulla creazione di un aggiornamento della struttura del database e sulla propagazione di tale modifica nell'intera applicazione Web.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungere una colonna
> * Aggiungere la proprietà alle visualizzazioni

## <a name="prerequisites"></a>Prerequisiti

* [Generazione di visualizzazioni](generating-views.md)

## <a name="add-a-column"></a>Aggiungere una colonna

Se si aggiorna la struttura di una tabella nel database, è necessario assicurarsi che la modifica venga propagata al modello di dati, alle visualizzazioni e al controller.

Per questa esercitazione verrà aggiunta una nuova colonna alla tabella Student per registrare il secondo nome dello studente. Per aggiungere questa colonna, aprire il progetto di database e aprire il file Student. SQL. Tramite la finestra di progettazione o il codice T-SQL, aggiungere una colonna denominata **MiddleName** che è di tipo nvarchar (50) e che consente valori null.

Distribuire questa modifica nel database locale avviando il progetto di database (o F5). Il nuovo campo viene aggiunto alla tabella. Se non viene visualizzato nella Esplora oggetti di SQL Server, fare clic sul pulsante Aggiorna nel riquadro.

![Mostra nuova colonna](changing-the-database/_static/image2.png)

La nuova colonna esiste nella tabella di database, ma attualmente non esiste nella classe del modello di dati. È necessario aggiornare il modello in modo da includere la nuova colonna. Nella cartella **Models (modelli** ) aprire il file **ContosoModel. edmx** per visualizzare il diagramma del modello. Si noti che il modello Student non contiene la proprietà MiddleName. Fare clic con il pulsante destro del mouse in un punto qualsiasi dell'area di progettazione e scegliere **Aggiorna modello da database**.

Nell'aggiornamento guidato selezionare la scheda **Aggiorna** e quindi selezionare **tabelle** > **dbo** > **Student**. Scegliere **Fine**.

Al termine del processo di aggiornamento, il diagramma di database include la nuova proprietà **MiddleName** . Salvare il file **ContosoModel. edmx** . È necessario salvare questo file affinché la nuova proprietà venga propagata alla classe **Student.cs** . A questo punto sono stati aggiornati il database e il modello.

Compilare la soluzione.

## <a name="add-the-property-to-the-views"></a>Aggiungere la proprietà alle visualizzazioni

Sfortunatamente, le visualizzazioni non contengono ancora la nuova proprietà. Per aggiornare le visualizzazioni sono disponibili due opzioni: è possibile rigenerare le viste aggiungendo di nuovo le impalcature per la classe Student oppure è possibile aggiungere manualmente la nuova proprietà alle visualizzazioni esistenti. In questa esercitazione si aggiungerà di nuovo l'impalcatura perché non sono state apportate modifiche personalizzate alle visualizzazioni generate automaticamente. È possibile considerare l'aggiunta manuale della proprietà quando sono state apportate modifiche alle visualizzazioni e non si desidera perdere tali modifiche.

Per assicurarsi che le visualizzazioni vengano ricreate, eliminare la cartella **students** in **views**ed eliminare il **StudentsController**. Quindi, fare clic con il pulsante destro del mouse sulla cartella **Controllers** e aggiungere l'impalcatura per il modello **Student** . Anche in questo caso, assegnare al controller il nome **StudentsController**. Selezionare **Aggiungi**.

Compilare nuovamente la soluzione. Le visualizzazioni ora contengono la proprietà MiddleName.

![Mostra il secondo nome](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiunta di una colonna
> * Aggiunta della proprietà alle visualizzazioni

Passare all'esercitazione successiva per informazioni su come personalizzare la visualizzazione per visualizzare i dettagli relativi a un record di studenti.
> [!div class="nextstepaction"]
> [Personalizzare una vista](customizing-a-view.md)