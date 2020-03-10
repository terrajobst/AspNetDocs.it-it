---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Pubblicare l'app nel servizio app Azure di Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622388"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a>Pubblicare l'app nel servizio app Azure di Azure

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://github.com/MikeWasson/BookService)

Come ultimo passaggio, l'applicazione viene pubblicata in Azure. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **pubblica**.

![](part-10/_static/image1.png)

Fare clic su **pubblica** per richiamare la finestra di dialogo **pubblica sul Web** . Se è stata selezionata l'opzione **host nel cloud** quando è stato creato per la prima volta il progetto, la connessione e le impostazioni sono già configurate. In tal caso, è sufficiente fare clic sulla scheda **Impostazioni** e selezionare &quot;esegui migrazioni Code First&quot;. Se all'inizio non è stata selezionata l'opzione **host nel cloud** , attenersi alla procedura descritta nella [sezione successiva](#new-website).

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Per distribuire l'app, fare clic su **pubblica**. È possibile visualizzare lo stato di avanzamento della pubblicazione nella finestra **attività di pubblicazione Web** . Scegliere **altre finestre**dal menu **Visualizza** , quindi selezionare **attività pubblicazione Web**.

![](part-10/_static/image4.png)

Quando Visual Studio completa la distribuzione dell'app, il browser predefinito si apre automaticamente all'URL del sito Web distribuito e l'applicazione creata viene ora eseguita nel cloud. L'URL nella barra degli indirizzi del browser indica che il sito è in fase di caricamento da Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Distribuzione in un nuovo sito Web

Se non è stata selezionata l'opzione **host nel cloud** al momento della creazione del progetto, è possibile configurare una nuova app Web. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **pubblica**. Selezionare la scheda **profilo** e fare clic su **Microsoft Azure siti Web**. Se non è attualmente stato effettuato l'accesso ad Azure, verrà richiesto di eseguire l'accesso.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

Nella finestra di dialogo **siti Web esistenti** fare clic su **nuovo**.

![](part-10/_static/image9.png)

Immettere un nome di sito. Selezionare la sottoscrizione di Azure e l'area. In **server database**selezionare **Crea nuovo server**o selezionare un server esistente. Scegliere **Crea**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Fare clic sulla scheda **Impostazioni** e selezionare &quot;esegui migrazioni Code First&quot;. Quindi viene selezionato **Pubblica**.

> [!div class="step-by-step"]
> [Precedente](part-9.md)
