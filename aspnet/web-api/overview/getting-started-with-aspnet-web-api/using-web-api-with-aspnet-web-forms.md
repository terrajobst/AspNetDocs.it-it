---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Uso dell'API Web con ASP.NET Web Forms-ASP.NET 4. x
author: MikeWasson
description: Esercitazione con codice Step by Step per aggiungere un'API Web a un'applicazione ASP.NET Forms per ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556777"
---
# <a name="using-web-api-with-aspnet-web-forms"></a>Usare l'API Web con Web Forms ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

Questa esercitazione illustra la procedura per aggiungere un'API Web a un'applicazione Web Forms ASP.NET tradizionale in ASP.NET 4. x. 

## <a name="overview"></a>Panoramica

Sebbene API Web ASP.NET sia incluso in un pacchetto con ASP.NET MVC, è facile aggiungere un'API Web a un'applicazione Web Form ASP.NET tradizionale.

Per usare l'API Web in un'applicazione Web Form, è necessario eseguire due passaggi principali:

- Aggiungere un controller API Web che deriva dalla classe **ApiController** .
- Aggiungere una tabella di route al metodo di **avvio dell'applicazione\_** .

## <a name="create-a-web-forms-project"></a>Creare un progetto Web Form

Avviare Visual Studio e selezionare **nuovo progetto** nella pagina **iniziale** . In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** . In **Visual C#** Selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **ASP.NET Web Forms Application**. Immettere un nome per il progetto e fare clic su **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Creare il modello e il controller

Questa esercitazione usa le stesse classi di modello e controller dell'esercitazione [Introduzione](tutorial-your-first-web-api.md) .

In primo luogo, aggiungere una classe modello. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi classe**. Denominare la classe Product e aggiungere l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Successivamente, aggiungere un controller API Web al progetto., un *controller* è l'oggetto che gestisce le richieste HTTP per l'API Web.

In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto. Selezionare **Aggiungi nuovo elemento**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

In **modelli installati**espandere **Visual C#**  e selezionare **Web**. Quindi, dall'elenco dei modelli, selezionare **classe controller API Web**. Assegnare al controller il nome "ProductsController" e fare clic su **Aggiungi**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

La procedura guidata **Aggiungi nuovo elemento** creerà un file denominato ProductsController.cs. Eliminare i metodi inclusi nella procedura guidata e aggiungere i metodi seguenti:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Per ulteriori informazioni sul codice in questo controller, vedere l'esercitazione [Introduzione](tutorial-your-first-web-api.md) .

## <a name="add-routing-information"></a>Aggiungi informazioni di routing

Successivamente, verrà aggiunta una route URI in modo che gli URI del modulo &quot;/API/Products/&quot; vengano indirizzati al controller.

In **Esplora soluzioni**fare doppio clic su Global. asax per aprire il file code-behind Global.asax.cs. Aggiungere la seguente istruzione **using** .

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Aggiungere quindi il codice seguente all' **applicazione\_metodo Start** :

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Per ulteriori informazioni sulle tabelle di routing, vedere [routing in API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Aggiungi AJAX sul lato client

È sufficiente creare un'API Web a cui i client possono accedere. A questo punto è possibile aggiungere una pagina HTML che usa jQuery per chiamare l'API.

Verificare che la pagina master (ad esempio, *site. master*) includa un `ContentPlaceHolder` con `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Aprire il file default. aspx. Sostituire il testo standard che si trova nella sezione contenuto principale, come illustrato:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Aggiungere quindi un riferimento al file di origine jQuery nella sezione `HeaderContent`:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Nota: è possibile aggiungere facilmente il riferimento allo script trascinando il file da **Esplora soluzioni** nella finestra dell'editor di codice.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Sotto il tag di script jQuery aggiungere il blocco di script seguente:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Quando il documento viene caricato, questo script esegue una richiesta AJAX per &quot;API/Products&quot;. La richiesta restituisce un elenco di prodotti in formato JSON. Lo script aggiunge le informazioni sul prodotto alla tabella HTML.

Quando si esegue l'applicazione, l'aspetto dovrebbe essere simile al seguente:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
