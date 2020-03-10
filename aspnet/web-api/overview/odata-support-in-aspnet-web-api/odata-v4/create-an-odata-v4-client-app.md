---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Creare un'app client OData V4 (C#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556294"
---
# <a name="create-an-odata-v4-client-app-c"></a>Creare un'app client OData v4 (C#)

di [Mike Wasson](https://github.com/MikeWasson)

Nell'esercitazione precedente è stato creato un servizio OData di base che supporta operazioni CRUD. A questo punto è possibile creare un client per il servizio.

Avviare una nuova istanza di Visual Studio e creare un nuovo progetto di applicazione console. Nella finestra di dialogo **nuovo progetto** selezionare **installato** &gt; **modelli** &gt; **Visual C#**  &gt; **desktop di Windows**e selezionare il modello **applicazione console** . Denominare il progetto &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> È anche possibile aggiungere l'app console alla stessa soluzione di Visual Studio che contiene il servizio OData.

## <a name="install-the-odata-client-code-generator"></a>Installare il generatore di codice client OData

Scegliere **Estensioni e aggiornamenti** dal menu **Strumenti**. Selezionare **Online** &gt; **Visual Studio Gallery**. Nella casella di ricerca cercare &quot;&quot;generatore di codice client OData. Fare clic su **download** per installare il progetto VSIX. Potrebbe essere richiesto di riavviare Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Eseguire il servizio OData localmente

Eseguire il progetto ProductService da Visual Studio. Per impostazione predefinita, Visual Studio avvia un browser per la radice dell'applicazione. Prendere nota dell'URI. Questa operazione sarà necessaria nel passaggio successivo. Lasciare l'applicazione in esecuzione.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Se si inseriscono entrambi i progetti nella stessa soluzione, assicurarsi di eseguire il progetto ProductService senza debug. Nel passaggio successivo sarà necessario eseguire il servizio durante la modifica del progetto di applicazione console.

## <a name="generate-the-service-proxy"></a>Generazione del proxy del servizio

Il proxy del servizio è una classe .NET che definisce i metodi per accedere al servizio OData. Il proxy converte le chiamate al metodo nelle richieste HTTP. Si creerà la classe proxy eseguendo un [modello T4](https://msdn.microsoft.com/library/bb126445.aspx).

Fare clic con il pulsante destro del mouse sul progetto. Selezionare **aggiungi** &gt; **nuovo elemento**.

![](create-an-odata-v4-client-app/_static/image5.png)

Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **elementi C# visivi** &gt; **codice** &gt; **client OData**. Denominare il modello &quot;ProductClient.tt&quot;. Fare clic su **Aggiungi** e quindi sull'avviso di sicurezza.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

A questo punto, si riceverà un errore, che può essere ignorato. Visual Studio esegue automaticamente il modello, ma il modello richiede prima alcune impostazioni di configurazione.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Aprire il file ProductClient. OData. config. Nell'elemento `Parameter` incollare l'URI dal progetto ProductService (passaggio precedente). Esempio:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Eseguire nuovamente il modello. In Esplora soluzioni, fare clic con il pulsante destro del mouse sul file ProductClient.tt e scegliere **Esegui strumento personalizzato**.

Il modello crea un file di codice denominato ProductClient.cs che definisce il proxy. Quando si sviluppa l'app, se si modifica l'endpoint OData, eseguire di nuovo il modello per aggiornare il proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Usare il proxy del servizio per chiamare il servizio OData

Aprire il file Program.cs e sostituire il codice standard con il codice seguente.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Sostituire il valore di *ServiceUri* con l'URI del servizio precedente.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Quando si esegue l'app, deve restituire quanto segue:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
