---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Scalabilità orizzontale di SignalR con il bus di servizio di Azure (SignalR 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e64f84db00b571c01ea52f48d1ac1af46698d391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558415"
---
# <a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Scale-out di SignalR con il bus di servizio di Azure (SignalR 1.x)

di [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

In questa esercitazione verrà distribuita un'applicazione SignalR a un ruolo Web di Windows Azure, usando il backplane del bus di servizio per distribuire i messaggi a ogni istanza del ruolo.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Prerequisiti:

- Un account Windows Azure.
- [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

Il backplane del bus di servizio è compatibile anche con il [bus di servizio per Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versione 1,1. Tuttavia, non è compatibile con la versione 1,0 del bus di servizio per Windows Server.

## <a name="pricing"></a>Prezzi

Il backplane del bus di servizio usa gli argomenti per inviare i messaggi. Per le informazioni più aggiornate sui prezzi, vedere [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). Al momento della stesura di questo articolo, è possibile inviare 1 milione messaggi al mese per meno di $1. Il backplane Invia un messaggio del bus di servizio per ogni chiamata di un metodo dell'hub SignalR. Sono inoltre presenti alcuni messaggi di controllo per le connessioni, le disconnessioni, il join o la uscita dei gruppi e così via. Nella maggior parte delle applicazioni, la maggior parte del traffico dei messaggi sarà chiamata al metodo dell'hub.

## <a name="overview"></a>Panoramica

Prima di arrivare all'esercitazione dettagliata, di seguito viene illustrata una rapida panoramica delle operazioni che si intende eseguire.

1. Usare il portale di Azure di Windows per creare un nuovo spazio dei nomi del bus di servizio.
2. Aggiungere i pacchetti NuGet all'applicazione: 

    - [Microsoft. AspNet. SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. SignalR. ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Creare un'applicazione SignalR.
4. Aggiungere il codice seguente a Global. asax per configurare il backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Per ogni applicazione, selezionare un valore diverso per "YourAppName". Non usare lo stesso valore tra più applicazioni.

## <a name="create-the-azure-services"></a>Creare i servizi di Azure

Creare un servizio cloud, come descritto in [come creare e distribuire un servizio cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Seguire i passaggi nella sezione "procedura: creare un servizio cloud usando creazione rapida". Per questa esercitazione non è necessario caricare un certificato.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Creare un nuovo spazio dei nomi del bus di servizio, come descritto in [come usare gli argomenti/sottoscrizioni del bus di servizio](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Seguire i passaggi nella sezione "creare uno spazio dei nomi del servizio".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Assicurarsi di selezionare la stessa area per il servizio cloud e lo spazio dei nomi del bus di servizio.

## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

Avviare Visual Studio. Scegliere **Nuovo progetto** dal menu **File**.

Nella finestra di dialogo **nuovo progetto** espandere oggetto **visivo C#** . In **modelli installati**selezionare **cloud** , quindi selezionare **servizio cloud di Microsoft Azure**. Mantieni il valore predefinito .NET Framework 4,5. Assegnare all'applicazione il nome ChatService e fare clic su **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

Nella finestra di dialogo **nuovo servizio cloud di Windows Azure** selezionare ruolo Web ASP.NET MVC 4. Fare clic sul pulsante freccia destra ( **&gt;** ) per aggiungere il ruolo alla soluzione.

Posizionare il puntatore del mouse sul nuovo ruolo, in modo che sia visibile l'icona della matita. Fare clic su questa icona per rinominare il ruolo. Assegnare al ruolo il nome "SignalRChat" e fare clic su **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

Nella creazione guidata **nuovo progetto ASP.NET MVC 4** selezionare **applicazione Internet**. Fare clic su **OK**. La creazione guidata progetto crea due progetti:

- ChatService: questo progetto è l'applicazione Windows Azure. Definisce i ruoli di Azure e altre opzioni di configurazione.
- SignalRChat: questo progetto è il progetto ASP.NET MVC 4.

## <a name="create-the-signalr-chat-application"></a>Creare l'applicazione di chat SignalR

Per creare l'applicazione di chat, seguire i passaggi descritti nell'esercitazione [Introduzione con SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Usare NuGet per installare le librerie necessarie. Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**. Nella finestra **console di gestione pacchetti** immettere i comandi seguenti:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Usare l'opzione `-ProjectName` per installare i pacchetti nel progetto MVC ASP.NET, invece che nel progetto Windows Azure.

## <a name="configure-the-backplane"></a>Configurare il backplane

Nel file Global. asax dell'applicazione aggiungere il codice seguente:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

A questo punto è necessario ottenere la stringa di connessione del bus di servizio. Nella portale di Azure selezionare lo spazio dei nomi del bus di servizio creato e fare clic sull'icona del tasto di scelta.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Copiare la stringa di connessione negli Appunti, quindi incollarla nella variabile *ConnectionString* .

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Distribuire in Azure

In Esplora soluzioni espandere la cartella **ruoli** all'interno del progetto ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Fare clic con il pulsante destro del mouse sul ruolo SignalRChat e scegliere **Proprietà**. Selezionare la scheda **configurazione** . In **istanze** selezionare 2. È anche possibile impostare le dimensioni della macchina virtuale su un numero molto **basso**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Salvare le modifiche.

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto ChatService. Selezionare **Pubblica**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Se è la prima volta che si esegue la pubblicazione in Windows Azure, è necessario scaricare le credenziali. Nella **pubblicazione** guidata, fare clic su "Accedi per scaricare le credenziali". Verrà richiesto di accedere al portale di Azure di Windows e di scaricare un file di impostazioni di pubblicazione.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Fare clic su **Importa** e selezionare il file di impostazioni di pubblicazione scaricato.

Fare clic su **Avanti**. Nella finestra di dialogo **impostazioni di pubblicazione** , in **servizio cloud**, selezionare il servizio cloud creato in precedenza.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Fare clic su **Pubblica**. La distribuzione dell'applicazione e l'avvio delle macchine virtuali potrebbero richiedere alcuni minuti.

Ora, quando si esegue l'applicazione di chat, le istanze del ruolo comunicano tramite il bus di servizio di Azure, usando un argomento del bus di servizio. Un argomento è una coda di messaggi che consente più Sottoscrittori.

Il backplane crea automaticamente l'argomento e le sottoscrizioni. Per visualizzare le sottoscrizioni e l'attività del messaggio, aprire il portale di Azure, selezionare lo spazio dei nomi del bus di servizio e fare clic su "argomenti".

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Per la visualizzazione dell'attività del messaggio nel dashboard sono necessari alcuni minuti.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR gestisce la durata dell'argomento. Finché l'applicazione viene distribuita, non provare a eliminare manualmente gli argomenti o a modificare le impostazioni dell'argomento.
