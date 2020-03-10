---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Creazione di un endpoint OData v3 con l'API Web 2 | Microsoft Docs
author: MikeWasson
description: Il Open Data Protocol (OData) è un protocollo di accesso ai dati per il Web. OData fornisce un modo uniforme per strutturare i dati, eseguire query sui dati e modificare i dati...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: e68a454398f109dfd089be9c9a44d3fe662acc2f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556413"
---
# <a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Creazione di un endpoint OData v3 con l'API Web 2

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Il [Open Data Protocol](http://www.odata.org/) (OData) è un protocollo di accesso ai dati per il Web. OData fornisce un modo uniforme per strutturare i dati, eseguire query sui dati e modificare il set di dati tramite operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione). OData supporta sia i formati AtomPub (XML) che JSON. OData definisce anche un modo per esporre i metadati relativi ai dati. I client possono utilizzare i metadati per individuare le informazioni sul tipo e le relazioni per il set di dati.
>
> API Web ASP.NET semplifica la creazione di un endpoint OData per un set di dati. È possibile controllare esattamente le operazioni OData supportate dall'endpoint. È possibile ospitare più endpoint OData, insieme a endpoint non OData. Si ha il controllo completo sul modello di dati, la logica di business back-end e il livello dati.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - API Web 2
> - OData versione 3
> - Entity Framework 6
> - [Proxy di debug Web Fiddler (facoltativo)](http://www.fiddler2.com)
>
> Il supporto OData dell'API Web è stato aggiunto nell' [aggiornamento ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Tuttavia, in questa esercitazione viene usata l'impalcatura aggiunta in Visual Studio 2013.

In questa esercitazione verrà creato un semplice endpoint OData su cui i client possono eseguire query. Si creerà anche un C# client per l'endpoint. Al termine dell'esercitazione, il set di esercitazioni successivo Mostra come aggiungere altre funzionalità, incluse le relazioni tra entità, le azioni e $expand/$select.

- [Creare il progetto di Visual Studio](#create-project)
- [Aggiungere un modello di entità](#add-model)
- [Aggiungere un controller OData](#add-controller)
- [Aggiungere EDM e Route](#edm)
- [Inizializzazione del database (facoltativo)](#seed-db)
- [Esplorazione dell'endpoint OData](#explore)
- [Formati di serializzazione OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

In questa esercitazione verrà creato un endpoint OData che supporta operazioni CRUD di base. L'endpoint esporrà un'unica risorsa, un elenco di prodotti. Nelle esercitazioni successive vengono aggiunte altre funzionalità.

Avviare Visual Studio e selezionare **nuovo progetto** nella pagina iniziale. In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo visivo C# . In **Visual C#** Selezionare **Web**. Selezionare **il modello applicazione Web ASP.NET** .

![](creating-an-odata-endpoint/_static/image1.png)

Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **vuoto** . In &quot;aggiungere cartelle e riferimenti principali per...&quot;, controllare l' **API Web**. Fare clic su **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Aggiungere un modello di entità

Un *modello* è un oggetto che rappresenta i dati nell'applicazione. Per questa esercitazione, è necessario un modello che rappresenti un prodotto. Il modello corrisponde al tipo di entità OData.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Modelli. Nel menu di scelta rapida selezionare **Aggiungi** e quindi selezionare **Classe**.

![](creating-an-odata-endpoint/_static/image3.png)

Nella finestra di dialogo **Aggiungi nuovo** elemento assegnare un nome alla classe &quot;&quot;prodotto.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Per convenzione, le classi del modello vengono inserite nella cartella Models. Non è necessario seguire questa convenzione nei propri progetti, ma verrà usata per questa esercitazione.

Nel file Product.cs aggiungere la definizione di classe seguente:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

La proprietà ID sarà la chiave di entità. I client possono eseguire query sui prodotti in base all'ID. Questo campo sarà anche la chiave primaria nel database back-end.

Compilare il progetto ora. Nel passaggio successivo verranno usate alcune impalcature di Visual Studio che usano la reflection per trovare il tipo di prodotto.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Aggiungere un controller OData

Un *controller* è una classe che gestisce le richieste HTTP. Si definisce un controller separato per ogni set di entità nel servizio OData. In questa esercitazione verrà creato un singolo controller.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Controllers. Selezionare **Aggiungi** e quindi selezionare **Controller**.

![](creating-an-odata-endpoint/_static/image5.png)

Nella finestra di dialogo **Aggiungi impalcatura** selezionare &quot;controller Web API 2 OData con azioni, usando Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

Nella finestra di dialogo **Aggiungi controller** assegnare al controller il nome "ProductsController". Selezionare la casella di controllo &quot;USA azioni del controller asincrono&quot;. Nell'elenco a discesa **modello** selezionare la classe Product.

![](creating-an-odata-endpoint/_static/image7.png)

Fare clic sul pulsante **nuovo contesto dati** . Lasciare il nome predefinito per il tipo di contesto dati e fare clic su **Aggiungi**.

![](creating-an-odata-endpoint/_static/image8.png)

Fare clic su Aggiungi nella finestra di dialogo Aggiungi controller per aggiungere il controller.

![](creating-an-odata-endpoint/_static/image9.png)

Nota: se viene visualizzato un messaggio di errore che indica &quot;si è verificato un errore durante il recupero del tipo...&quot;, assicurarsi di aver compilato il progetto di Visual Studio dopo aver aggiunto la classe Product. L'impalcatura usa la reflection per trovare la classe.

![](creating-an-odata-endpoint/_static/image10.png)

Le impalcature aggiungono due file di codice al progetto:

- Products.cs definisce il controller API Web che implementa l'endpoint OData.
- ProductServiceContext.cs fornisce metodi per eseguire query sul database sottostante, utilizzando Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Aggiungere EDM e Route

In Esplora soluzioni espandere l'app\_cartella di avvio e aprire il file denominato WebApiConfig.cs. Questa classe include il codice di configurazione per l'API Web. Sostituire il codice con il codice seguente:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Questo codice esegue due operazioni:

- Crea un Entity Data Model (EDM) per l'endpoint OData.
- Aggiunge una route per l'endpoint.

EDM è un modello astratto dei dati. EDM viene utilizzato per creare il documento di metadati e definire gli URI per il servizio. **ODataConventionModelBuilder** crea un modello EDM utilizzando un set di convenzioni di denominazione predefinite EDM. Questo approccio richiede il minor codice. Se si desidera un maggiore controllo sul modello EDM, è possibile utilizzare la classe **ODataModelBuilder** per creare EDM aggiungendo proprietà, chiavi e proprietà di navigazione in modo esplicito.

Il metodo **EntitySet** aggiunge un set di entità a EDM:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

La stringa "Products" definisce il nome del set di entità. Il nome del controller deve corrispondere al nome del set di entità. In questa esercitazione il set di entità è denominato "Products" e il controller è denominato `ProductsController`. Se è stato denominato il set di entità "Products", assegnare al controller il nome `ProductSetController`. Si noti che un endpoint può avere più set di entità. Chiamare **EntitySet&lt;t&gt;** per ogni set di entità e quindi definire un controller corrispondente.

Il metodo **MapODataRoute** aggiunge una route per l'endpoint OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

Il primo parametro è un nome descrittivo per la route. I client del servizio non visualizzano questo nome. Il secondo parametro è il prefisso URI per l'endpoint. Dato questo codice, l'URI per il set di entità Products è http://<em>hostname</em>/OData/Products. L'applicazione può avere più di un endpoint OData. Per ogni endpoint, chiamare <strong>MapODataRoute</strong> e fornire un nome di route univoco e un prefisso URI univoco.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Inizializzazione del database (facoltativo)

In questo passaggio si utilizzerà Entity Framework per inizializzare il database con alcuni dati di test. Questo passaggio è facoltativo, ma consente di testare immediatamente l'endpoint OData.

Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**. Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Verrà aggiunta una cartella denominata Migrations e un file di codice denominato Configuration.cs.

![](creating-an-odata-endpoint/_static/image12.png)

Aprire il file e aggiungere il codice seguente al metodo `Configuration.Seed`.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Nella finestra console di gestione pacchetti immettere i comandi seguenti:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Questi comandi generano codice che crea il database, quindi esegue tale codice.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Esplorazione dell'endpoint OData

In questa sezione verrà usato il proxy di [debug Web Fiddler](http://www.fiddler2.com) per inviare richieste all'endpoint ed esaminare i messaggi di risposta. Ciò consentirà di comprendere le funzionalità di un endpoint OData.

In Visual Studio premere F5 per avviare il debug. Per impostazione predefinita, Visual Studio apre il browser per `http://localhost:*port*`, dove *porta* è il numero di porta configurato nelle impostazioni del progetto.

È possibile modificare il numero di porta nelle impostazioni del progetto. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**. Nella finestra Proprietà selezionare **Web**. Immettere il numero di porta in **URL progetto**.

### <a name="service-document"></a>Documento di servizio

Il *documento di servizio* contiene un elenco dei set di entità per l'endpoint OData. Per ottenere il documento di servizio, inviare una richiesta GET all'URI radice del servizio.

Utilizzando Fiddler, immettere l'URI seguente nella scheda **Composer** : `http://localhost:port/odata/`, dove *porta* è il numero di porta.

![](creating-an-odata-endpoint/_static/image13.png)

Fare clic sul pulsante **Esegui** . Fiddler invia una richiesta HTTP GET all'applicazione. Verrà visualizzata la risposta nell'elenco sessioni Web. Se tutto funziona, il codice di stato sarà 200.

![](creating-an-odata-endpoint/_static/image14.png)

Fare doppio clic sulla risposta nell'elenco sessioni Web per visualizzare i dettagli del messaggio di risposta nella scheda controlli.

![](creating-an-odata-endpoint/_static/image15.png)

Il messaggio di risposta HTTP non elaborato dovrebbe essere simile al seguente:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Per impostazione predefinita, l'API Web restituisce il documento di servizio in formato AtomPub. Per richiedere JSON, aggiungere l'intestazione seguente alla richiesta HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

A questo punto la risposta HTTP contiene un payload JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Documento metadati del servizio

Nel *documento dei metadati del servizio* viene descritto il modello di dati del servizio utilizzando un linguaggio XML denominato CSDL (Conceptual Schema Definition Language). Il documento dei metadati Mostra la struttura dei dati nel servizio e può essere usato per generare il codice client.

Per ottenere il documento di metadati, inviare una richiesta GET a `http://localhost:port/odata/$metadata`. Ecco i metadati per l'endpoint illustrato in questa esercitazione.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Set di entità

Per ottenere il set di entità Products, inviare una richiesta GET a `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entità

Per ottenere un singolo prodotto, inviare una richiesta GET a `http://localhost:port/odata/Products(1)`, dove "1" è l'ID prodotto.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formati di serializzazione OData

OData supporta diversi formati di serializzazione:

- Pub Atom (XML)
- JSON "Light" (introdotto in OData v3)
- JSON "verbose" (OData v2)

Per impostazione predefinita, l'API Web usa il formato AtomPubJSON "Light".

Per ottenere il formato AtomPub, impostare l'intestazione Accept su "Application/Atom + XML". Di seguito è riportato il corpo di una risposta di esempio:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

È possibile notare uno svantaggio evidente del formato Atom: è molto più dettagliato della luce JSON. Tuttavia, se si dispone di un client che riconosce AtomPub, il client potrebbe preferire tale formato tramite JSON.

Ecco la versione JSON Light della stessa entità:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Il formato JSON Light è stato introdotto nella versione 3 del protocollo OData. Per compatibilità con le versioni precedenti, un client può richiedere il formato JSON "verbose" meno recente. Per richiedere il formato JSON dettagliato, impostare l'intestazione Accept su `application/json;odata=verbose`. La versione dettagliata è la seguente:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Questo formato fornisce un numero maggiore di metadati nel corpo della risposta, che può aggiungere un sovraccarico notevole su un'intera sessione. Inoltre, aggiunge un livello di riferimento indiretto eseguendo il wrapping dell'oggetto in una proprietà denominata "d".

## <a name="next-steps"></a>Passaggi successivi

- [Aggiungi relazioni tra entità](working-with-entity-relations.md)
- [Aggiungi azioni OData](odata-actions.md)
- [Chiamare il servizio OData da un client .NET](calling-an-odata-service-from-a-net-client.md)
