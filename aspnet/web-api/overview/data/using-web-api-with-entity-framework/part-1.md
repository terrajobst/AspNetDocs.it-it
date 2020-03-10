---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Uso dell'API Web 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra le nozioni di base per la creazione di un'applicazione Web con un back-end API Web ASP.NET. L'esercitazione USA Entity Framework 6 per i dati Lay...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622479"
---
# <a name="using-web-api-2-with-entity-framework-6"></a>Uso dell'API Web 2 con Entity Framework 6

[Scarica progetto completato](https://github.com/MikeWasson/BookService)

> Questa esercitazione illustra le nozioni di base per la creazione di un'applicazione Web con un back-end API Web ASP.NET. L'esercitazione USA Entity Framework 6 per il livello dati ed knockout. js per l'applicazione JavaScript sul lato client. L'esercitazione illustra anche come distribuire l'app in app Web del servizio app Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
> - API Web 2,1
> - Visual Studio 2017 (scaricare Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
> - [Knockout. js](http://knockoutjs.com/) 3,1

Questa esercitazione USA API Web ASP.NET 2 con Entity Framework 6 per creare un'applicazione Web che manipola un database back-end. Di seguito è riportata una schermata dell'applicazione che verrà creata.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

L'app usa una progettazione di applicazioni a singola pagina (SPA). "Applicazione a pagina singola" è il termine generale per un'applicazione Web che carica una singola pagina HTML, quindi aggiorna la pagina in modo dinamico, anziché caricare nuove pagine. Al termine del caricamento iniziale della pagina, l'app comunica con il server tramite richieste AJAX. Le richieste AJAX restituiscono i dati JSON, usati dall'app per aggiornare l'interfaccia utente.

AJAX non è una novità, ma oggi sono disponibili Framework JavaScript che semplificano la creazione e la gestione di un'applicazione SPA sofisticata di grandi dimensioni. Questa esercitazione USA [knockout. js](http://knockoutjs.com/), ma è possibile usare qualsiasi framework client JavaScript.

Ecco i principali blocchi predefiniti per questa app:

- ASP.NET MVC crea la pagina HTML.
- API Web ASP.NET gestisce le richieste AJAX e restituisce i dati JSON.
- Dati knockout. js: associa gli elementi HTML ai dati JSON.
- Entity Framework comunica con il database.

## <a name="see-this-app-running-on-azure"></a>Vedi questa app in esecuzione in Azure

Si desidera visualizzare il sito finito in esecuzione come app Web Live? È possibile distribuire una versione completa dell'app nell'account Azure selezionando il pulsante seguente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Per distribuire questa soluzione in Azure, è necessario un account Azure. Se non si dispone già di un account, sono disponibili le opzioni seguenti:

- [Apri un account Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) gratuitamente: puoi ottenere crediti che puoi usare per provare i servizi di Azure a pagamento e, anche dopo che sono stati usati, puoi tenere l'account e usare i servizi di Azure gratuiti.
- [Attiva i vantaggi per gli abbonati MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : l'abbonamento MSDN ti offre crediti ogni mese che puoi usare per i servizi di Azure a pagamento.

## <a name="create-the-project"></a>Creare il progetto

Aprire Visual Studio. Scegliere **nuovo**dal menu **file** , quindi selezionare **progetto**. (Oppure selezionare **nuovo progetto** nella pagina iniziale).

Nella finestra di dialogo **nuovo progetto** selezionare **Web** nel riquadro a sinistra e **ASP.NET Web Application (.NET Framework)** nel riquadro centrale. Assegnare al progetto il nome **BookService** e selezionare **OK**.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **API Web** .

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

Selezionare **OK** per creare il progetto.

## <a name="configure-azure-settings-optional"></a>Configurare le impostazioni di Azure (facoltativo)

Dopo aver creato il progetto, è possibile scegliere di eseguire la distribuzione in app Azure app Web del servizio in qualsiasi momento. 

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **pubblica**.

2. Nella finestra visualizzata selezionare **Avvia**. Verrà visualizzata la finestra di dialogo **Seleziona destinazione di pubblicazione** .

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Selezionare **Crea profilo**. Viene visualizzata la finestra di dialogo **Crea servizio app**.

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Accettare le impostazioni predefinite o immettere valori diversi per il nome dell'applicazione, il gruppo di risorse, il piano di hosting, la sottoscrizione di Azure e l'area geografica. 

4. Selezionare **Crea un database SQL**. Verrà visualizzata la finestra di dialogo **configura SQL Server** . 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Accettare le impostazioni predefinite o immettere valori diversi. Immettere un **nome utente amministratore** e una **password amministratore** per il nuovo database. Al termine, fare clic su **OK** . Viene nuovamente visualizzata la pagina **Crea servizio app** .

5. Selezionare **Crea** per creare il profilo. Viene visualizzato un messaggio nell'angolo in basso a destra che indica che la distribuzione è in corso. Dopo un breve periodo di tempo, la finestra di **pubblicazione** viene nuovamente visualizzata.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    Il profilo creato per la distribuzione dell'app è ora disponibile. 

> [!div class="step-by-step"]
> [avanti](part-2.md)
