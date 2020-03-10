---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Creare un endpoint OData V4 usando API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: Il Open Data Protocol (OData) è un protocollo di accesso ai dati per il Web. OData fornisce un modo uniforme per eseguire query e modificare i set di dati tramite operazioni CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598735"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a>Creare un endpoint OData V4 usando API Web ASP.NET 

> Il Open Data Protocol (OData) è un protocollo di accesso ai dati per il Web. OData fornisce un modo uniforme per eseguire query e modificare i set di dati tramite operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione).
>
> API Web ASP.NET supporta sia V3 che V4 del protocollo. È anche possibile avere un endpoint V4 eseguito side-by-side con un endpoint V3.
>
> Questa esercitazione illustra come creare un endpoint OData V4 che supporta operazioni CRUD.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
> - API Web 5,2
> - OData v4
> - Visual Studio 2017 (scaricare Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/))
> - Entity Framework 6
> - .NET 4.7.2
>
> ## <a name="tutorial-versions"></a>Versioni di esercitazione
>
> Per la versione 3 di OData, vedere [creazione di un endpoint OData v3](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

In Visual Studio scegliere **nuovo** &gt; **progetto**dal menu **file** .

Espandere **installato** &gt; **Visual C#**  &gt; **Web**e selezionare il modello **applicazione Web ASP.NET (.NET Framework)** . Denominare il progetto &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

Selezionare **OK**.

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

Selezionare il modello **Vuoto**. In **Aggiungi cartelle e riferimenti principali per:** selezionare **API Web**. Selezionare **OK**.

## <a name="install-the-odata-packages"></a>Installare i pacchetti OData

Dal menu **strumenti** selezionare **gestione pacchetti NuGet** &gt; console di **Gestione pacchetti**. Nella finestra console di gestione pacchetti, digitare:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Questo comando installa i pacchetti NuGet OData più recenti.

## <a name="add-a-model-class"></a>Aggiungere una classe modello

Un *modello* è un oggetto che rappresenta un'entità di dati nell'applicazione.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella Modelli. Dal menu di scelta rapida selezionare **aggiungi** &gt; **classe**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Per convenzione, le classi del modello vengono inserite nella cartella Models, ma non è necessario seguire questa convenzione nei propri progetti.

Assegnare alla classe il nome `Product`. Nel file Product.cs sostituire il codice standard con il codice seguente:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

La proprietà `Id` è la chiave di entità. I client possono eseguire query sulle entità in base alla chiave. Per ottenere, ad esempio, il prodotto con ID 5, l'URI viene `/Products(5)`. La proprietà `Id` sarà anche la chiave primaria nel database back-end.

## <a name="enable-entity-framework"></a>Abilita Entity Framework

Per questa esercitazione si userà Entity Framework (EF) Code First per creare il database back-end.

> [!NOTE]
> L'API Web OData non richiede EF. Utilizzare qualsiasi livello di accesso ai dati in grado di convertire entità di database in modelli.

Per prima cosa, installare il pacchetto NuGet per EF. Dal menu **strumenti** selezionare **gestione pacchetti NuGet** &gt; console di **Gestione pacchetti**. Nella finestra console di gestione pacchetti, digitare:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Aprire il file Web. config e aggiungere la sezione seguente all'interno dell'elemento di **configurazione** , dopo l'elemento **configSections** .

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Questa impostazione consente di aggiungere una stringa di connessione per un database del database locale. Questo database verrà usato quando si esegue l'app in locale.

Successivamente, aggiungere una classe denominata `ProductsContext` alla cartella Models:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

Nel costruttore `"name=ProductsContext"` assegna il nome della stringa di connessione.

## <a name="configure-the-odata-endpoint"></a>Configurare l'endpoint OData

Aprire l'app file\_Start/WebApiConfig. cs. Aggiungere le istruzioni **using** seguenti:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Aggiungere quindi il codice seguente al metodo **Register** :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Questo codice esegue due operazioni:

- Crea un Entity Data Model (EDM).
- Aggiunge una route.

EDM è un modello astratto dei dati. EDM viene utilizzato per creare il documento di metadati del servizio. La classe **ODataConventionModelBuilder** crea un modello EDM usando le convenzioni di denominazione predefinite. Questo approccio richiede il minor codice. Se si desidera un maggiore controllo sul modello EDM, è possibile utilizzare la classe **ODataModelBuilder** per creare EDM aggiungendo proprietà, chiavi e proprietà di navigazione in modo esplicito.

Una *Route* indica all'API Web come instradare le richieste HTTP all'endpoint. Per creare una route OData V4, chiamare il metodo di estensione **MapODataServiceRoute** .

Se l'applicazione dispone di più endpoint OData, creare una route separata per ognuno di essi. Assegnare a ogni route un nome di route univoco e un prefisso.

## <a name="add-the-odata-controller"></a>Aggiungere il controller OData

Un *controller* è una classe che gestisce le richieste HTTP. Si crea un controller separato per ogni set di entità nel servizio OData. In questa esercitazione verrà creato un controller per l'entità `Product`.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella controller e scegliere **aggiungi** &gt; **classe**. Assegnare alla classe il nome `ProductsController`.

> [!NOTE]
> La versione di questa esercitazione per OData v3 usa l' **aggiunta** di impalcature del controller. Attualmente non esiste alcuna impalcatura per OData V4.

Sostituire il codice standard in ProductsController.cs con il codice seguente.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Il controller utilizza la classe `ProductsContext` per accedere al database tramite EF. Si noti che il controller esegue l'override del metodo **Dispose** per eliminare il **ProductsContext**.

Questo è il punto di partenza per il controller. Successivamente, verranno aggiunti i metodi per tutte le operazioni CRUD.

## <a name="query-the-entity-set"></a>Eseguire una query sul set di entità

Aggiungere i metodi seguenti per `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

La versione senza parametri del metodo `Get` restituisce l'intera raccolta di prodotti. Il `Get` metodo con un parametro *chiave* Cerca un prodotto in base alla relativa chiave, in questo caso la proprietà `Id`.

L'attributo **[EnableQuery]** consente ai client di modificare la query tramite opzioni di query quali $filter, $sort e $Page. Per ulteriori informazioni, vedere [supporto delle opzioni di query OData](../supporting-odata-query-options.md).

## <a name="add-an-entity-to-the-entity-set"></a>Aggiungere un'entità al set di entità

Per consentire ai client di aggiungere un nuovo prodotto al database, aggiungere il metodo seguente per `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a>Aggiornare un'entità

OData supporta due diversi tipi di semantica per l'aggiornamento di un'entità, PATCH e PUT.

- PATCH esegue un aggiornamento parziale. Il client specifica solo le proprietà da aggiornare.
- PUT sostituisce l'intera entità.

Lo svantaggio di PUT è che il client deve inviare i valori per tutte le proprietà nell'entità, inclusi i valori che non sono in corso di modifica. La [specifica OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indica che è preferibile la patch.

In ogni caso, di seguito è riportato il codice per i metodi PATCH e PUT:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

Nel caso di PATCH, il controller utilizza il tipo **Delta&lt;t&gt;** per tenere traccia delle modifiche.

## <a name="delete-an-entity"></a>Eliminare un'entità

Per consentire ai client di eliminare un prodotto dal database, aggiungere il metodo seguente per `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
