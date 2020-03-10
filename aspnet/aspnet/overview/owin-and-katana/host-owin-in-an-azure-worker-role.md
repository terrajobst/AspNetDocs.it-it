---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Ospitare OWIN in un ruolo di lavoro di Azure | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come ospitare autonomamente OWIN in un ruolo di lavoro Microsoft Azure. Open Web Interface for .NET (OWIN) definisce un'astrazione tra il server Web .NET...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 59d2e0d549427093f8a2424b17af81169b78ef30
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584616"
---
# <a name="host-owin-in-an-azure-worker-role"></a>Ospitare OWIN in un ruolo di lavoro di Azure

di [Mike Wasson](https://github.com/MikeWasson)

> Questa esercitazione illustra come ospitare autonomamente OWIN in un ruolo di lavoro Microsoft Azure.
>
> [Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server Web .NET e le applicazioni Web. OWIN separa l'applicazione Web dal server, rendendo OWIN ideale per l'hosting automatico di un'applicazione Web nel proprio processo, al di fuori di IIS, ad esempio all'interno di un ruolo di lavoro di Azure.
>
> In questa esercitazione si apprenderà come ospitare autonomamente le applicazioni OWIN all'interno di un ruolo di lavoro Microsoft Azure. Per altre informazioni sui ruoli di lavoro, vedere [modelli di esecuzione di Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [Azure SDK per .NET 2,3](https://azure.microsoft.com/downloads/)
> - [Microsoft. Owin. SelfHost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)

## <a name="create-a-microsoft-azure-project"></a>Creare un progetto di Microsoft Azure

Avviare Visual Studio con privilegi di amministratore. I privilegi di amministratore sono necessari per eseguire il debug dell'applicazione in locale usando l'emulatore di calcolo di Azure.

Scegliere **nuovo**dal menu **file** , quindi fare clic su **progetto**. Da **modelli installati**, in Visual C#fare clic su **cloud** , quindi su **servizio cloud di Microsoft Azure**. Denominare il progetto "AzureApp" e fare clic su **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

Nella finestra di dialogo **nuovo servizio cloud di Microsoft Azure** fare doppio clic su **ruolo di lavoro**. Lasciare il nome predefinito ("WorkerRole1"). Questo passaggio consente di aggiungere un ruolo di lavoro alla soluzione. Fare clic su **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

La soluzione di Visual Studio creata contiene due progetti:

- &quot;AzureApp&quot; definisce i ruoli e la configurazione per l'applicazione Azure.
- &quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.

In generale, un'applicazione Azure può contenere più ruoli, anche se in questa esercitazione viene usato un singolo ruolo.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Aggiungere i pacchetti Self-host OWIN

Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet**e quindi su **console di gestione pacchetti**.

Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Aggiungere un endpoint HTTP

In Esplora soluzioni espandere il progetto AzureApp. Espandere il nodo ruoli, fare clic con il pulsante destro del mouse su WorkerRole1 e scegliere **Proprietà**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Scegliere **Endpoint**, quindi fare clic su **Aggiungi endpoint**.

Nell'elenco a discesa **protocollo** selezionare "http". In **porta pubblica** e **porta privata**digitare 80. Questi numeri di porta possono essere diversi. La porta pubblica è quella usata dai client per inviare una richiesta al ruolo.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Creare la classe di avvio OWIN

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto WorkerRole1 e selezionare **aggiungi** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `Startup`.

Sostituire tutto il codice standard con quanto segue:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

Il metodo di estensione `UseWelcomePage` aggiunge una semplice pagina HTML all'applicazione per verificare il corretto funzionamento del sito.

## <a name="start-the-owin-host"></a>Avviare l'host OWIN

Aprire il file WorkerRole.cs. Questa classe definisce il codice che viene eseguito quando il ruolo di lavoro viene avviato e arrestato.

Aggiungere l'istruzione using seguente:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Aggiungere un membro **IDisposable** alla classe `WorkerRole`:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

Nel metodo `OnStart` aggiungere il codice seguente per avviare l'host:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

Il metodo **webapp. Start** avvia l'host OWIN. Il nome della classe `Startup` è un parametro di tipo per il metodo. Per convenzione, l'host chiamerà il metodo `Configure` di questa classe.

Eseguire l'override del `OnStop` per eliminare l'istanza dell' *app\_* :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Ecco il codice completo per WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Compilare la soluzione e premere F5 per eseguire l'applicazione localmente nell'emulatore di calcolo di Azure. A seconda delle impostazioni del firewall, potrebbe essere necessario consentire all'emulatore di usare il firewall.

L'emulatore di calcolo assegna un indirizzo IP locale all'endpoint. È possibile trovare l'indirizzo IP visualizzando l'interfaccia utente dell'emulatore di calcolo. Fare clic con il pulsante destro del mouse sull'icona dell'emulatore nell'area di notifica della barra delle applicazioni e scegliere **Mostra interfaccia utente emulatore di calcolo**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Trovare l'indirizzo IP in distribuzioni del servizio, distribuzione [ID], Dettagli servizio. Aprire un Web browser e passare a http:\/*indirizzo*\/, dove *Indirizzo* è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80`. Verrà visualizzata la pagina iniziale di OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Distribuire in Azure

Per questo passaggio, è necessario disporre di un account Azure. Se non si dispone già di un account, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [Microsoft Azure versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto AzureApp. Selezionare **Pubblica**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Se non è stato effettuato l'accesso al proprio account Azure, fare clic su **Accedi**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Dopo aver eseguito l'accesso, scegliere una sottoscrizione e fare clic su **Avanti**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Immettere un nome per il servizio cloud e scegliere un'area. Scegliere **Crea**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Fare clic su **Pubblica**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

La finestra log attività di Azure Mostra lo stato di avanzamento della distribuzione. Quando si distribuisce l'app, passare a `http://appname.cloudapp.net/`, dove *appname* è il nome del servizio cloud.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Panoramica del progetto Katana](an-overview-of-project-katana.md)
- [Progetto Katana su GitHub](https://github.com/aspnet/AspNetKatana/)
