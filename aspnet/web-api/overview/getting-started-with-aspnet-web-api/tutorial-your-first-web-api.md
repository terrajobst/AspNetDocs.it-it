---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introduzione a API Web ASP.NET 2 (C#)-ASP.NET 4. x
author: MikeWasson
description: Esercitazione con codice. Usare API Web ASP.NET per creare un'API Web che restituisce un elenco di prodotti.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2717d93f47be9d4a6548731d8deeca312b25f39f
ms.sourcegitcommit: 9e3ca74997a67c18589729d4b7303799905473eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084049"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a>Introduzione a API Web ASP.NET 2 (C#)

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

In questa esercitazione si userà API Web ASP.NET per creare un'API Web che restituisce un elenco di prodotti.

Il protocollo HTTP non è solo per le pagine Web di servizio. HTTP è anche una potente piattaforma per la creazione di API che espongono servizi e dati. HTTP è semplice, flessibile e onnipresente. Quasi tutte le piattaforme che è possibile considerare hanno una libreria HTTP, quindi i servizi HTTP possono raggiungere un'ampia gamma di client, inclusi browser, dispositivi mobili e applicazioni desktop tradizionali.

API Web ASP.NET è un Framework per la creazione di API Web su .NET Framework. 

## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- API Web 2

Per una versione più recente di questa esercitazione, vedere [creare un'API Web con ASP.NET Core e Visual Studio per Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) .

## <a name="create-a-web-api-project"></a>Creare un progetto API Web

In questa esercitazione si userà API Web ASP.NET per creare un'API Web che restituisce un elenco di prodotti. La pagina Web front-end utilizza jQuery per visualizzare i risultati.

![](tutorial-your-first-web-api/_static/image1.png)

Avviare Visual Studio e selezionare **nuovo progetto** nella pagina **iniziale** . In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** . In **Visual C#** Selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET**. Denominare il progetto "ProductsApp" e fare clic su **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **vuoto** . In &quot;aggiungere cartelle e riferimenti principali per&quot;, selezionare **API Web**. Fare clic su **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> È anche possibile creare un progetto API Web usando il modello di&quot; API Web di &quot;. Il modello API Web USA ASP.NET MVC per fornire le pagine della Guida dell'API. Sto usando il modello vuoto per questa esercitazione perché voglio visualizzare l'API Web senza MVC. In generale, non è necessario essere a conoscenza di ASP.NET MVC per usare l'API Web.

## <a name="adding-a-model"></a>Aggiunta di un modello

Un *modello* è un oggetto che rappresenta i dati nell'applicazione. API Web ASP.NET possibile serializzare automaticamente il modello in JSON, XML o in un altro formato, quindi scrivere i dati serializzati nel corpo del messaggio di risposta HTTP. Finché un client può leggere il formato di serializzazione, può deserializzare l'oggetto. La maggior parte dei client può analizzare XML o JSON. Inoltre, il client può indicare il formato desiderato impostando l'intestazione Accept nel messaggio di richiesta HTTP.

Iniziamo creando un modello semplice che rappresenta un prodotto.

Se Esplora soluzioni non è ancora visibile, fare clic sul menu **Visualizza** e quindi selezionare **Esplora soluzioni**. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Modelli. Nel menu di scelta rapida selezionare **Aggiungi** e quindi selezionare **Classe**.

![](tutorial-your-first-web-api/_static/image4.png)

Denominare la classe &quot;&quot;prodotto. Aggiungere le proprietà seguenti alla classe `Product`.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Aggiunta di un controller

Nell'API Web un *controller* è un oggetto che gestisce richieste HTTP. Si aggiungerà un controller che può restituire un elenco di prodotti o un singolo prodotto specificato dall'ID.

> [!NOTE]
> Se è stato usato ASP.NET MVC, si ha già familiarità con i controller. I controller API Web sono simili ai controller MVC, ma ereditano la classe **ApiController** invece della classe **controller** .

In **Esplora soluzioni** fare clic sulla cartella Controller. Selezionare **Aggiungi** e quindi selezionare **Controller**.

![](tutorial-your-first-web-api/_static/image5.png)

Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Controller Web API 2 - Vuoto**. Fare clic su **Aggiungi**.

![](tutorial-your-first-web-api/_static/image6.png)

Nella finestra di dialogo **Aggiungi controller** assegnare un nome al controller &quot;ProductsController&quot;. Fare clic su **Aggiungi**.

![](tutorial-your-first-web-api/_static/image7.png)

Tramite l'impalcatura viene creato un file denominato ProductsController.cs nella cartella Controllers.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Non è necessario inserire i controller in una cartella denominata Controllers. Il nome della cartella è semplicemente un modo pratico per organizzare i file di origine.

Se non è già aperto, fare doppio clic sul file per aprirlo. Sostituire il codice in questo file con quanto segue:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Per semplificare l'esempio, i prodotti vengono archiviati in una matrice fissa all'interno della classe controller. Naturalmente, in un'applicazione reale, si esegue una query su un database o si utilizza un'altra origine dati esterna.

Il controller definisce due metodi che restituiscono i prodotti:

- Il metodo `GetAllProducts` restituisce l'intero elenco di prodotti come **IEnumerable&lt;tipo di&gt;prodotto** .
- Il metodo `GetProduct` Cerca un singolo prodotto in base al relativo ID.

L'operazione è terminata. Si dispone di un'API Web funzionante. Ogni metodo sul controller corrisponde a uno o più URI:

| Controller (metodo) | URI |
| --- | --- |
| GetAllProducts | /api/products |
| GetProduct | *ID* /API/Products/ |

Per il metodo `GetProduct`, l' *ID* nell'URI è un segnaposto. Per ottenere, ad esempio, il prodotto con ID 5, l'URI viene `api/products/5`.

Per ulteriori informazioni su come l'API Web instrada le richieste HTTP ai metodi del controller, vedere [routing in API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Chiamata dell'API Web con JavaScript e jQuery

In questa sezione verrà aggiunta una pagina HTML che usa AJAX per chiamare l'API Web. Si userà jQuery per eseguire le chiamate AJAX e aggiornare anche la pagina con i risultati.

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, quindi selezionare **nuovo elemento**.

![](tutorial-your-first-web-api/_static/image9.png)

Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il nodo **Web** in **oggetto C#visivo** , quindi selezionare l'elemento della **pagina HTML** . Assegnare alla pagina il nome &quot;&quot;index. html.

![](tutorial-your-first-web-api/_static/image10.png)

Sostituire tutti gli elementi in questo file con i seguenti elementi:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

È possibile ottenere jQuery in vari modi. In questo esempio è stata utilizzata la rete [CDN Microsoft AJAX](../../../ajax/cdn/overview.md). È anche possibile scaricarlo da [http://jquery.com/](http://jquery.com/)e il modello di progetto "API Web" di ASP.NET include anche jQuery.

### <a name="getting-a-list-of-products"></a>Ottenere un elenco di prodotti

Per ottenere un elenco di prodotti, inviare una richiesta HTTP GET a &quot;&quot;/API/Products.

La funzione jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) invia una richiesta AJAX. La risposta contiene una matrice di oggetti JSON. La funzione `done` specifica un callback che viene chiamato se la richiesta ha esito positivo. Nel callback, il DOM viene aggiornato con le informazioni sul prodotto.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Recupero di un prodotto in base all'ID

Per ottenere un prodotto in base all'ID, inviare una richiesta HTTP GET a &quot;*ID* /API/Products/&quot;, dove *ID* è l'ID prodotto.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Si chiama ancora `getJSON` per inviare la richiesta AJAX, ma questa volta si inserisce l'ID nell'URI della richiesta. La risposta da questa richiesta è una rappresentazione JSON di un singolo prodotto.

## <a name="running-the-application"></a>Esecuzione dell'applicazione

Premere F5 per avviare il debug dell'applicazione. La pagina Web dovrebbe essere simile alla seguente:

![](tutorial-your-first-web-api/_static/image11.png)

Per ottenere un prodotto in base all'ID, immettere l'ID e fare clic su Cerca:

![](tutorial-your-first-web-api/_static/image12.png)

Se si immette un ID non valido, il server restituisce un errore HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Uso di F12 per visualizzare la richiesta e la risposta HTTP

Quando si utilizza un servizio HTTP, può essere molto utile visualizzare i messaggi di richiesta e risposta HTTP. A tale scopo, è possibile usare gli strumenti di sviluppo F12 in Internet Explorer 9. Da Internet Explorer 9 Premere **F12** per aprire gli strumenti. Fare clic sulla scheda **rete** e premere **Avvia acquisizione**. Tornare quindi alla pagina Web e premere **F5** per ricaricare la pagina Web. Internet Explorer acquisirà il traffico HTTP tra il browser e il server Web. La visualizzazione riepilogo Mostra tutto il traffico di rete per una pagina:

![](tutorial-your-first-web-api/_static/image14.png)

Individuare la voce relativa all'URI relativo "API/Products/". Selezionare questa voce e fare clic su **Vai a visualizzazione dettagliata**. Nella visualizzazione dettagli sono disponibili schede per visualizzare le intestazioni e i corpi della richiesta e della risposta. Se, ad esempio, si fa clic sulla scheda **intestazioni richiesta** , è possibile osservare che il client ha richiesto &quot;&quot; Application/JSON nell'intestazione Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Se si fa clic sulla scheda corpo della risposta, è possibile visualizzare il modo in cui l'elenco di prodotti è stato serializzato in JSON. Altri browser hanno funzionalità simili. Un altro strumento utile è [Fiddler](http://www.fiddler2.com/fiddler2/), un proxy di debug Web. È possibile usare Fiddler per visualizzare il traffico HTTP e anche per comporre richieste HTTP, che offrono il controllo completo sulle intestazioni HTTP nella richiesta.

## <a name="see-this-app-running-on-azure"></a>Vedi questa app in esecuzione in Azure

Si desidera visualizzare il sito finito in esecuzione come app Web Live? È possibile distribuire una versione completa dell'app nell'account Azure semplicemente facendo clic sul pulsante seguente.

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Per distribuire questa soluzione in Azure, è necessario un account Azure. Se non si dispone già di un account, sono disponibili le opzioni seguenti:

- [Apri un account Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) gratuitamente: puoi ottenere crediti che puoi usare per provare i servizi di Azure a pagamento e, anche dopo che sono stati usati, puoi tenere l'account e usare i servizi di Azure gratuiti.
- [Attiva i vantaggi per gli abbonati MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : l'abbonamento MSDN ti offre crediti ogni mese che puoi usare per i servizi di Azure a pagamento.

## <a name="next-steps"></a>Passaggi successivi

- Per un esempio più completo di un servizio HTTP che supporta le operazioni POST, PUT e DELETE e le Scritture in un database, vedere [uso dell'API Web 2 con Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Per altre informazioni sulla creazione di applicazioni Web fluide e reattive su un servizio HTTP, vedere [applicazione a pagina singola ASP.NET](../../../single-page-application/index.md).
- Per informazioni su come distribuire un progetto Web di Visual Studio nel servizio app Azure, vedere [creare un'app web ASP.NET in app Azure Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
