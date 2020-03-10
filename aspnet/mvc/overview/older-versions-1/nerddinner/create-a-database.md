---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Creare un database | Microsoft Docs
author: microsoft
description: Nel passaggio 2 sono illustrati i passaggi per creare il database contenente tutti i dati di Dinner and RSVP per l'applicazione NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581004"
---
# <a name="create-a-database"></a>Creare un database

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 2 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.
> 
> Nel passaggio 2 sono illustrati i passaggi per creare il database contenente tutti i dati di Dinner and RSVP per l'applicazione NerdDinner.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner passaggio 2: creazione del database

Verrà usato un database per archiviare tutti i dati di Dinner and RSVP per l'applicazione NerdDinner.

I passaggi seguenti illustrano la creazione del database usando l'edizione gratuita di SQL Server Express (che è possibile installare facilmente usando V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)). Tutto il codice che scriviamo funziona sia con SQL Server Express che con l'SQL Server completo.

### <a name="creating-a-new-sql-server-express-database"></a>Creazione di un nuovo database di SQL Server Express

Si inizia facendo clic con il pulsante destro del mouse sul progetto Web e quindi scegliendo il comando di menu **aggiungi&gt;nuovo elemento** :

![](create-a-database/_static/image1.png)

Verrà visualizzata la finestra di dialogo "Aggiungi nuovo elemento" di Visual Studio. Si filtra in base alla categoria "data" e si seleziona il modello di elemento "SQL Server database":

![](create-a-database/_static/image2.png)

Denominare il database SQL Server Express si vuole creare "NerdDinner. mdf" e fare clic su OK. Visual Studio chiederà se si desidera aggiungere questo file alla directory \app\_data (ovvero una directory già impostata con ACL di sicurezza di lettura e scrittura):

![](create-a-database/_static/image3.png)

Si farà clic su "Sì" e il nuovo database verrà creato e aggiunto al Esplora soluzioni:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Creazione di tabelle nel database

È ora disponibile un nuovo database vuoto. È ora possibile aggiungere alcune tabelle.

A tale scopo, si passerà alla finestra della scheda "Esplora server" all'interno di Visual Studio, che consente di gestire i database e i server. SQL Server Express database archiviati nella cartella \App\_data dell'applicazione verranno visualizzati automaticamente all'interno del Esplora server. Facoltativamente, è possibile usare l'icona "Connetti al database" nella parte superiore della finestra "Esplora server" per aggiungere anche ulteriori database SQL Server (sia locali che remoti) all'elenco:

![](create-a-database/_static/image5.png)

Nel database NerdDinner vengono aggiunte due tabelle, una per archiviare le cene e l'altra per tenere traccia delle accettazioni RSVP. È possibile creare nuove tabelle facendo clic con il pulsante destro del mouse sulla cartella "tabelle" all'interno del database e scegliendo il comando di menu "Aggiungi nuova tabella":

![](create-a-database/_static/image6.png)

Verrà aperta una finestra di progettazione tabelle che consente di configurare lo schema della tabella. Per la tabella "cene" si aggiungeranno 10 colonne di dati:

![](create-a-database/_static/image7.png)

Si vuole che la colonna "DinnerID" sia una chiave primaria univoca per la tabella. È possibile configurarlo facendo clic con il pulsante destro del mouse sulla colonna "DinnerID" e scegliendo la voce di menu "Imposta chiave primaria":

![](create-a-database/_static/image8.png)

Oltre a rendere DinnerID una chiave primaria, è necessario configurarla anche come colonna "Identity", il cui valore viene incrementato automaticamente man mano che vengono aggiunte nuove righe di dati alla tabella, ovvero la prima riga inserita Dinner avrà DinnerID 1, la seconda riga inserita. il valore di DinnerID sarà 2 e così via.

A tale scopo, selezionare la colonna "DinnerID" e quindi utilizzare l'editor "Proprietà colonna" per impostare la proprietà "(identità)" della colonna su "Sì". Si utilizzeranno le impostazioni predefinite di identità standard (a partire da 1 e Increment 1 per ogni nuova riga di Dinner):

![](create-a-database/_static/image9.png)

La tabella verrà quindi salvata digitando CTRL + S o usando il comando di menu **file-&gt;Salva** . Verrà richiesto di assegnare un nome alla tabella. Denominarlo "dinners":

![](create-a-database/_static/image10.png)

La nuova tabella dinners verrà visualizzata nel database in Esplora server.

Si ripeteranno quindi i passaggi precedenti e si creerà una tabella "RSVP". Questa tabella con tre colonne. Si imposta la colonna RsvpID come chiave primaria e la si imposta anche come colonna Identity:

![](create-a-database/_static/image11.png)

Lo salviamo e diamo il nome "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Impostazione di una relazione di chiave esterna tra tabelle

Sono ora disponibili due tabelle nel database. L'ultima fase di progettazione dello schema consiste nell'impostare una relazione "uno-a-molti" tra queste due tabelle, in modo da poter associare ogni riga della cena a zero o più righe RSVP che vi si applicano. Questa operazione viene eseguita configurando la colonna "DinnerID" della tabella RSVP per avere una relazione di chiave esterna con la colonna "DinnerID" nella tabella "dinners".

A tale scopo, verrà aperta la tabella RSVP in Progettazione tabelle facendo doppio clic su di essa in Esplora server. Selezionare quindi la colonna "DinnerID" al suo interno, fare clic con il pulsante destro del mouse e scegliere "relazioni". comando del menu di scelta rapida:

![](create-a-database/_static/image12.png)

Verrà visualizzata una finestra di dialogo che è possibile usare per configurare le relazioni tra le tabelle:

![](create-a-database/_static/image13.png)

Si farà clic sul pulsante "Aggiungi" per aggiungere una nuova relazione alla finestra di dialogo. Una volta aggiunta una relazione, espandere il nodo della visualizzazione albero "tabelle e colonne" nella griglia delle proprietà a destra della finestra di dialogo, quindi fare clic su "...". a destra del pulsante:

![](create-a-database/_static/image14.png)

Fare clic su "..." verrà visualizzata un'altra finestra di dialogo che consente di specificare le tabelle e le colonne che interessano la relazione, oltre a consentire di assegnare un nome alla relazione.

La tabella della chiave primaria verrà modificata in "dinners" e la colonna "DinnerID" nella tabella dinners verrà selezionata come chiave primaria. La tabella RSVP sarà la tabella di chiave esterna e l'operazione RSVP. La colonna DinnerID verrà associata come chiave esterna:

![](create-a-database/_static/image15.png)

Ogni riga della tabella RSVP verrà ora associata a una riga nella tabella dinner. SQL Server manterrà l'integrità referenziale per gli Stati Uniti e ci impedirà di aggiungere una nuova riga RSVP se non punta a una riga di cena valida. Si eviterà inoltre di eliminare una riga di Dinner se vi sono ancora righe RSVP che vi fanno riferimento.

### <a name="adding-data-to-our-tables"></a>Aggiunta di dati alle tabelle

Per completare l'aggiunta di alcuni dati di esempio, vedere la tabella dinners. È possibile aggiungere dati a una tabella facendovi clic con il pulsante destro del mouse all'interno del Esplora server e scegliendo il comando "Mostra dati tabella":

![](create-a-database/_static/image16.png)

Verranno aggiunte alcune righe di dati di Dinner che è possibile usare in un secondo momento, quando si inizia a implementare l'applicazione:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Passo successivo

La creazione del database è stata completata. A questo punto è possibile creare le classi del modello che è possibile usare per eseguire query e aggiornarle.

> [!div class="step-by-step"]
> [Precedente](create-a-new-aspnet-mvc-project.md)
> [Successivo](build-a-model-with-business-rule-validations.md)
