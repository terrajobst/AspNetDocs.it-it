---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Usare OWIN per ospitare autonomamente API Web ASP.NET-ASP.NET 4. x
author: rick-anderson
description: Esercitazione con codice che illustra come ospitare API Web ASP.NET in un'applicazione console.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556539"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a>Usare OWIN per ospitare autonomamente API Web ASP.NET 

> Questa esercitazione illustra come ospitare API Web ASP.NET in un'applicazione console, usando OWIN per ospitare autonomamente il Framework API Web.
>
> [Open Web Interface for .NET](http://owin.org) (OWIN) definisce un'astrazione tra i server Web .NET e le applicazioni Web. OWIN separa l'applicazione Web dal server, rendendo OWIN ideale per l'hosting automatico di un'applicazione Web nel proprio processo, al di fuori di IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - API Web 5.2.7

> [!NOTE]
> È possibile trovare il codice sorgente completo per questa esercitazione in [github.com/AspNet/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).

## <a name="create-a-console-application"></a>Creare un'applicazione console

Scegliere **nuovo**dal menu **file** , quindi **progetto**. Da **installato**, in **Visual C#** selezionare **desktop di Windows** , quindi selezionare **app console (.NET Framework)** . Denominare il progetto "OwinSelfhostSample" e fare clic su **OK**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Aggiungere l'API Web e i pacchetti OWIN

Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**. Nella finestra Console di gestione pacchetti immettere il comando seguente:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Verrà installato il pacchetto WebAPI OWIN SelfHost e tutti i pacchetti OWIN richiesti.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurare l'API Web per l'hosting indipendente

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **aggiungi** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Sostituire tutto il codice standard in questo file con il codice seguente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Aggiungere un controller API Web

Aggiungere quindi una classe controller API Web. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **aggiungi** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `ValuesController`.

Sostituire tutto il codice standard in questo file con il codice seguente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Avviare l'host OWIN ed effettuare una richiesta con HttpClient

Sostituire tutto il codice standard nel file Program.cs con il codice seguente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Esecuzione dell'applicazione

Per eseguire l'applicazione, premere F5 in Visual Studio. L'output dovrebbe essere simile al seguente:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Risorse aggiuntive

[Panoramica del progetto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Ospitare API Web ASP.NET in un ruolo di lavoro di Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
