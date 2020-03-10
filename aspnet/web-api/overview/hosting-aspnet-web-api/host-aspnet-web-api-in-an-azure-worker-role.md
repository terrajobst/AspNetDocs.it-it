---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Ospitare API Web ASP.NET 2 in un ruolo di lavoro di Azure-ASP.NET 4. x
author: MikeWasson
description: 'Esercitazione: ospitare API Web ASP.NET in un ruolo di lavoro di Azure, usando OWIN per ospitare autonomamente il Framework API Web.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: ec9904e0bff090be0f504036ae73977cfca0cb31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556630"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Ospitare API Web ASP.NET 2 in un ruolo di lavoro di Azure

di [Mike Wasson](https://github.com/MikeWasson)

> Questa esercitazione illustra come ospitare API Web ASP.NET in un ruolo di lavoro di Azure, usando OWIN per ospitare autonomamente il Framework API Web.
>
> [Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server Web .NET e le applicazioni Web. OWIN separa l'applicazione Web dal server, rendendo OWIN ideale per l'hosting automatico di un'applicazione Web nel proprio processo, al di fuori di IIS, ad esempio all'interno di un ruolo di lavoro di Azure.
>
> In questa esercitazione si userà il pacchetto Microsoft. Owin. host. HttpListener, che fornisce un server HTTP che verrà usato per ospitare le applicazioni OWIN in modo indipendente.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - API Web 2
> - [Azure SDK per .NET 2,3](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a>Creare un progetto di Microsoft Azure

Avviare Visual Studio con privilegi di amministratore. I privilegi di amministratore sono necessari per eseguire il debug dell'applicazione in locale usando l'emulatore di calcolo di Azure.

Scegliere **nuovo**dal menu **file** , quindi fare clic su **progetto**. Da **modelli installati**, in Visual C#fare clic su **cloud** , quindi su **servizio cloud di Microsoft Azure**. Denominare il progetto "AzureApp" e fare clic su **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

Nella finestra di dialogo **nuovo servizio cloud di Microsoft Azure** fare doppio clic su **ruolo di lavoro**. Lasciare il nome predefinito ("WorkerRole1"). Questo passaggio consente di aggiungere un ruolo di lavoro alla soluzione. Fare clic su **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

La soluzione di Visual Studio creata contiene due progetti:

- &quot;AzureApp&quot; definisce i ruoli e la configurazione per l'applicazione Azure.
- &quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.

In generale, un'applicazione Azure può contenere più ruoli, anche se in questa esercitazione viene usato un singolo ruolo.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Aggiungere l'API Web e i pacchetti OWIN

Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet**e quindi su **console di gestione pacchetti**.

Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Aggiungere un endpoint HTTP

In Esplora soluzioni espandere il progetto AzureApp. Espandere il nodo ruoli, fare clic con il pulsante destro del mouse su WorkerRole1 e scegliere **Proprietà**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Scegliere **Endpoint**, quindi fare clic su **Aggiungi endpoint**.

Nell'elenco a discesa **protocollo** selezionare "http". In **porta pubblica** e **porta privata**digitare 80. Questi numeri di porta possono essere diversi. La porta pubblica è quella usata dai client per inviare una richiesta al ruolo.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Configurare l'API Web per l'hosting indipendente

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto WorkerRole1 e selezionare **aggiungi** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Sostituire tutto il codice standard in questo file con il codice seguente:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Aggiungere un controller API Web

Aggiungere quindi una classe controller API Web. Fare clic con il pulsante destro del mouse sul progetto WorkerRole1 e scegliere **aggiungi** / **classe**. Denominare la classe TestController. Sostituire tutto il codice standard in questo file con il codice seguente:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Per semplicità, questo controller definisce solo due metodi GET che restituiscono testo normale.

## <a name="start-the-owin-host"></a>Avviare l'host OWIN

Aprire il file WorkerRole.cs. Questa classe definisce il codice che viene eseguito quando il ruolo di lavoro viene avviato e arrestato.

Aggiungere l'istruzione using seguente:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Aggiungere un membro **IDisposable** alla classe `WorkerRole`:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

Nel metodo `OnStart` aggiungere il codice seguente per avviare l'host:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

Il metodo **webapp. Start** avvia l'host OWIN. Il nome della classe `Startup` è un parametro di tipo per il metodo. Per convenzione, l'host chiamerà il metodo `Configure` di questa classe.

Eseguire l'override del `OnStop` per eliminare l'istanza dell' *app\_* :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Ecco il codice completo per WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Compilare la soluzione e premere F5 per eseguire l'applicazione localmente nell'emulatore di calcolo di Azure. A seconda delle impostazioni del firewall, potrebbe essere necessario consentire all'emulatore di usare il firewall.

> [!NOTE]
> Se viene generata un'eccezione simile alla seguente, vedere [questo post di Blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) per una soluzione alternativa. "Impossibile caricare il file o l'assembly ' Microsoft. Owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle relative dipendenze. La definizione del manifesto dell'assembly individuato non corrisponde al riferimento all'assembly. (Eccezione da HRESULT: 0x80131040) "

L'emulatore di calcolo assegna un indirizzo IP locale all'endpoint. È possibile trovare l'indirizzo IP visualizzando l'interfaccia utente dell'emulatore di calcolo. Fare clic con il pulsante destro del mouse sull'icona dell'emulatore nell'area di notifica della barra delle applicazioni e scegliere **Mostra interfaccia utente emulatore di calcolo**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Trovare l'indirizzo IP in distribuzioni del servizio, distribuzione [ID], Dettagli servizio. Aprire un Web browser e passare all'<em>indirizzo</em>http:///test/1, dove <em>Indirizzo</em> è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80/test/1`. Verrà visualizzata la risposta del controller API Web:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Distribuire in Azure

Per questo passaggio, è necessario disporre di un account Azure. Se non si dispone già di un account, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [Microsoft Azure versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto AzureApp. Selezionare **Pubblica**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Se non è stato effettuato l'accesso al proprio account Azure, fare clic su **Accedi**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Dopo aver eseguito l'accesso, scegliere una sottoscrizione e fare clic su **Avanti**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Immettere un nome per il servizio cloud e scegliere un'area. Scegliere **Crea**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Fare clic su **Pubblica**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

La finestra log attività di Azure Mostra lo stato di avanzamento della distribuzione. Quando si distribuisce l'app, passare a http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Risorse aggiuntive

- [Panoramica del progetto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Progetto Katana su GitHub](https://github.com/aspnet/AspNetKatana)
