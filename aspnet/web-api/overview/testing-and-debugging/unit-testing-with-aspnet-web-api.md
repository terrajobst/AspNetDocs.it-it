---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Testing unità API Web ASP.NET 2 | Microsoft Docs
author: Rick-Anderson
description: Questa guida e l'applicazione illustrano come creare semplici unit test per l'applicazione Web API 2. Questa esercitazione illustra come includere un unit test proj...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f2d60b977475e048a3a74aabff4adc768ee22baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554971"
---
# <a name="unit-testing-aspnet-web-api-2"></a>Testing unità API Web ASP.NET 2

di [Tom FitzMacken](https://github.com/tfitzmac)

[Scarica progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Questa guida e l'applicazione illustrano come creare semplici unit test per l'applicazione Web API 2. Questa esercitazione illustra come includere un progetto unit test nella soluzione e scrivere metodi di test che controllano i valori restituiti da un metodo controller.
>
> Questa esercitazione presuppone che l'utente abbia familiarità con i concetti di base di API Web ASP.NET. Per un'esercitazione introduttiva, vedere [Introduzione con API Web ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> Gli unit test di questo argomento sono intenzionalmente limitati a scenari di dati semplici. Per lo unit test di scenari di dati più avanzati, vedere [Entity Framework di simulazione durante l'unit test API Web ASP.NET 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2

## <a name="in-this-topic"></a>In questo argomento

Questo argomento è suddiviso nelle sezioni seguenti:

- [Prerequisiti](#prereqs)
- [Scarica codice](#download)
- [Creare un'applicazione con unit test progetto](#appwithunittest)
    - [Aggiungere unit test progetto durante la creazione dell'applicazione](#whencreate)
    - [Aggiungere unit test progetto a un'applicazione esistente](#addtoexisting)
- [Configurare l'applicazione Web API 2](#setupproject)
- [Installare i pacchetti NuGet nel progetto di test](#testpackages)
- [Crea test](#tests)
- [Esegui test](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Prerequisiti

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Scaricare il codice

Scaricare il [progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Il progetto scaricabile include unit test codice per questo argomento e per il [Entity Framework fittizio quando si verificano unit test API Web ASP.NET](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) argomento.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Creare un'applicazione con unit test progetto

È possibile creare un progetto unit test durante la creazione dell'applicazione o aggiungere un progetto di unit test a un'applicazione esistente. In questa esercitazione vengono illustrati entrambi i metodi per la creazione di un progetto unit test. Per seguire questa esercitazione, è possibile usare entrambi gli approcci.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Aggiungere unit test progetto durante la creazione dell'applicazione

Creare una nuova applicazione Web ASP.NET denominata **StoreApp**.

![crea progetto](unit-testing-with-aspnet-web-api/_static/image1.png)

Nelle finestre del nuovo progetto ASP.NET selezionare il modello **vuoto** e aggiungere cartelle e riferimenti principali per l'API Web. Selezionare l'opzione **Aggiungi unit test** . Il progetto unit test viene automaticamente denominato **StoreApp. tests**. Questo nome può essere mantenuto.

![Crea unit test progetto](unit-testing-with-aspnet-web-api/_static/image2.png)

Dopo aver creato l'applicazione, si noterà che contiene due progetti.

![due progetti](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Aggiungere unit test progetto a un'applicazione esistente

Se non è stato creato il progetto di unit test quando è stata creata l'applicazione, è possibile aggiungerne uno in qualsiasi momento. Si supponga, ad esempio, di avere già un'applicazione denominata StoreApp e di voler aggiungere unit test. Per aggiungere un progetto di unit test, fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Aggiungi** e **nuovo progetto**.

![Aggiungi nuovo progetto alla soluzione](unit-testing-with-aspnet-web-api/_static/image4.png)

Selezionare **test** nel riquadro a sinistra e selezionare **progetto unit test** per il tipo di progetto. Denominare il progetto **StoreApp. tests**.

![Aggiungi unit test progetto](unit-testing-with-aspnet-web-api/_static/image5.png)

Il progetto unit test sarà visualizzato nella soluzione.

Nel progetto unit test aggiungere un riferimento al progetto originale.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Configurare l'applicazione Web API 2

Nel progetto StoreApp aggiungere un file di classe alla cartella **Models** denominata **Product.cs**. Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Compilare la soluzione.

Fare clic con il pulsante destro del mouse sulla cartella controller e scegliere **Aggiungi** e **nuovo elemento con impalcatura**. Selezionare **Controller Web API 2-vuoto**.

![Aggiungi nuovo controller](unit-testing-with-aspnet-web-api/_static/image6.png)

Impostare il nome del controller su **SimpleProductController**, quindi fare clic su **Aggiungi**.

![specificare il controller](unit-testing-with-aspnet-web-api/_static/image7.png)

Sostituire il codice esistente con quello seguente. Per semplificare questo esempio, i dati vengono archiviati in un elenco anziché in un database. L'elenco definito in questa classe rappresenta i dati di produzione. Si noti che il controller include un costruttore che accetta come parametro un elenco di oggetti Product. Questo costruttore consente di passare dati di test durante il testing unità. Il controller include anche due metodi **asincroni** per illustrare gli unit test dei metodi asincroni. Questi metodi asincroni sono stati implementati chiamando **Task. FromResult** per ridurre al minimo il codice estraneo, ma in genere i metodi includono operazioni con utilizzo intensivo delle risorse.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Il metodo GetProduct restituisce un'istanza dell'interfaccia **IHttpActionResult** . IHttpActionResult è una delle nuove funzionalità dell'API Web 2 e semplifica lo sviluppo di unit test. Le classi che implementano l'interfaccia IHttpActionResult si trovano nello spazio dei nomi [System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) . Queste classi rappresentano le possibili risposte da una richiesta di azione e corrispondono ai codici di stato HTTP.

Compilare la soluzione.

A questo punto è possibile configurare il progetto di test.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installare i pacchetti NuGet nel progetto di test

Quando si usa il modello vuoto per creare un'applicazione, il progetto unit test (StoreApp. tests) non include i pacchetti NuGet installati. Altri modelli, ad esempio il modello API Web, includono alcuni pacchetti NuGet nel progetto unit test. Per questa esercitazione, è necessario includere il pacchetto di Microsoft ASP.NET Web API 2 core nel progetto di test.

Fare clic con il pulsante destro del mouse sul progetto StoreApp. tests e scegliere **Gestisci pacchetti NuGet**. Per aggiungere i pacchetti al progetto, è necessario selezionare il progetto StoreApp. tests.

![Gestisci pacchetti](unit-testing-with-aspnet-web-api/_static/image8.png)

Trovare e installare Microsoft ASP.NET pacchetto Web API 2 Core.

![installare il pacchetto di base dell'API Web](unit-testing-with-aspnet-web-api/_static/image9.png)

Chiudere la finestra Gestisci pacchetti NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Creare test

Per impostazione predefinita, il progetto di test include un file di test vuoto denominato UnitTest1.cs. Questo file Mostra gli attributi usati per creare i metodi di test. Per gli unit test, è possibile usare questo file o creare un file personalizzato.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

Per questa esercitazione verrà creata una classe di test personalizzata. È possibile eliminare il file UnitTest1.cs. Aggiungere una classe denominata **TestSimpleProductController.cs**e sostituire il codice con il codice seguente.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Esegui test

A questo punto è possibile eseguire i test. Verranno testati tutti i metodi contrassegnati con l'attributo **TestMethod** . Dalla voce di menu **test** eseguire i test.

![eseguire test](unit-testing-with-aspnet-web-api/_static/image11.png)

Aprire la finestra **Esplora test** e osservare i risultati dei test.

![risultati test](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Riepilogo

L'esercitazione è stata completata. I dati in questa esercitazione sono stati intenzionalmente semplificati per concentrarsi sulle condizioni di testing unità. Per lo unit test di scenari di dati più avanzati, vedere [Entity Framework di simulazione durante l'unit test API Web ASP.NET 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
