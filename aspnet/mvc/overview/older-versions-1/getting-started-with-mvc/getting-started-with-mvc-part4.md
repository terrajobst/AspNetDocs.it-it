---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Creazione di un database | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581438"
---
# <a name="creating-a-database"></a>Creazione di un database

di [Scott hanseln](https://github.com/shanselman)

> Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Verrà creata una semplice applicazione Web che legge e scrive da un database. Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.

In questa sezione verrà creato un nuovo database di SQL Express che verrà usato per archiviare e recuperare i dati dei film. Dall'IDE di Visual Web Developer selezionare Visualizza | Esplora server. Fare clic con il pulsante destro del mouse su connessioni dati e scegliere Aggiungi connessione...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Nella finestra di dialogo Scegli origine dati selezionare Microsoft SQL Server e selezionare continua.

![](getting-started-with-mvc-part4/_static/image2.png)

Nella finestra di dialogo Aggiungi connessione immettere ".\SQLEXPRESS" per il nome del server e immettere "Movies" come nome per il nuovo database.

[![finestra di dialogo Aggiungi connessione](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Fare clic su OK. verrà chiesto se si desidera creare il database. Selezionare Sì.

[![creare film?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

A questo punto si ha un database vuoto in Esplora server.

![Aggiungi nuova tabella](getting-started-with-mvc-part4/_static/image7.png)

Fare clic con il pulsante destro del mouse su tabelle e scegliere Aggiungi tabella. Verrà visualizzata la Progettazione tabelle. Aggiungere colonne per ID, titolo, rilasciato, genere e prezzo. Fare clic con il pulsante destro del mouse sulla colonna ID e scegliere Imposta chiave primaria. Ecco le aree di progettazione che mi sembrano.

[Editor tabelle di database ![](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Inoltre, selezionare la colonna ID e in proprietà colonna sotto impostare "specifica identità" su "Sì".

[![le proprietà della colonna Identity](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Al termine, fare clic sull'icona Salva sulla barra degli strumenti o selezionare file | Salvare dal menu e denominare la tabella "**Movie**" (singolare). Sono presenti un database e una tabella.

[![scegliere il nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Tornare alla Esplora server e fare clic con il pulsante destro del mouse sulla tabella Movie, quindi selezionare "Mostra dati tabella". Immettere alcuni filmati in modo che il database disponga di alcuni dati.

[![modifica della tabella di database](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Creazione di un modello

Tornare ora alla Esplora soluzioni sul lato destro dell'IDE e fare clic con il pulsante destro del mouse sulla cartella Models e scegliere Aggiungi | Nuovo elemento.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Verrà creato un modello di entità dal nuovo database. Verrà aggiunto un set di classi al progetto che semplifica la query e la manipolazione dei dati all'interno del database. Selezionare il nodo dati nella parte sinistra della finestra di dialogo e quindi selezionare il modello ADO.NET Entity Data Model elemento. Denominarlo Movies. edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Fare clic sul pulsante "Aggiungi". Verrà quindi avviata la procedura guidata "Entity Data Model".

Nella nuova finestra di dialogo visualizzata selezionare Genera da database. Poiché è stato appena creato un database, sarà necessario indicare solo le Entity Framework sul nuovo database e sulla relativa tabella. Fare clic su Avanti per salvare la connessione al database nella configurazione dell'applicazione Web. A questo punto, selezionare la casella di controllo tabelle e film, quindi fare clic su fine.

[Procedura guidata ![Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

È ora possibile visualizzare la nuova tabella Movie nel Entity Framework Designer e accedervi dal codice.

[![Movies-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Nell'area di progettazione è possibile visualizzare una classe "film". Questa classe esegue il mapping alla tabella "Movie" nel database e ogni proprietà all'interno di essa esegue il mapping a una colonna con la tabella. Ogni istanza di una classe "film" corrisponderà a una riga all'interno della tabella "Movie".

Se non si desiderano le convenzioni di denominazione e mapping predefinite utilizzate dal Entity Framework, è possibile utilizzare la finestra di progettazione Entity Framework per modificarle o personalizzarle. Per questa applicazione verranno usate le impostazioni predefinite e il file verrà salvato così com'è.

Ora è possibile usare alcuni dati reali.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part3.md)
> [Successivo](getting-started-with-mvc-part5.md)
