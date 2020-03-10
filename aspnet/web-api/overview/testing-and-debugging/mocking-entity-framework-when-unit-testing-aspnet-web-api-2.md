---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Entity Framework fittizio durante l'unit test API Web ASP.NET 2 | Microsoft Docs
author: Rick-Anderson
description: Questa guida e l'applicazione illustrano come creare unit test per l'applicazione Web API 2 che usa la Entity Framework. Mostra come modificare...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555090"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Entity Framework fittizio durante il testing unità API Web ASP.NET 2

di [Tom FitzMacken](https://github.com/tfitzmac)

[Scarica progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Questa guida e l'applicazione illustrano come creare unit test per l'applicazione Web API 2 che usa la Entity Framework. Viene illustrato come modificare il controller con impalcature per consentire il passaggio di un oggetto di contesto per il test e come creare oggetti di test che funzionano con Entity Framework.
>
> Per un'introduzione agli unit test con API Web ASP.NET, vedere [Unit Testing with API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md).
>
> Questa esercitazione presuppone che l'utente abbia familiarità con i concetti di base di API Web ASP.NET. Per un'esercitazione introduttiva, vedere [Introduzione con API Web ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
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
- [Creare la classe modello](#modelclass)
- [Aggiungere il controller](#controller)
- [Aggiungi inserimento delle dipendenze](#dependency)
- [Installare i pacchetti NuGet nel progetto di test](#testpackages)
- [Crea contesto di test](#testcontext)
- [Crea test](#tests)
- [Esegui test](#runtests)

Se sono già stati completati i passaggi in [unit test con API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), è possibile passare alla sezione [aggiungere il controller](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Prerequisiti

Visual Studio 2017 community, Professional o Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Scaricare il codice

Scaricare il [progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Il progetto scaricabile include unit test codice per questo argomento e per l'argomento [unit testing API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md) .

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Creare un'applicazione con unit test progetto

È possibile creare un progetto unit test durante la creazione dell'applicazione o aggiungere un progetto di unit test a un'applicazione esistente. Questa esercitazione illustra la creazione di un progetto unit test durante la creazione dell'applicazione.

Creare una nuova applicazione Web ASP.NET denominata **StoreApp**.

Nelle finestre del nuovo progetto ASP.NET selezionare il modello **vuoto** e aggiungere cartelle e riferimenti principali per l'API Web. Selezionare l'opzione **Aggiungi unit test** . Il progetto unit test viene automaticamente denominato **StoreApp. tests**. Questo nome può essere mantenuto.

![Crea unit test progetto](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Dopo aver creato l'applicazione, si noterà che contiene due progetti: **StoreApp** e **StoreApp. tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Creare la classe modello

Nel progetto StoreApp aggiungere un file di classe alla cartella **Models** denominata **Product.cs**. Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Compilare la soluzione.

<a id="controller"></a>
## <a name="add-the-controller"></a>Aggiungere il controller

Fare clic con il pulsante destro del mouse sulla cartella controller e scegliere **Aggiungi** e **nuovo elemento con impalcatura**. Selezionare controller Web API 2 con azioni, usando Entity Framework.

![Aggiungi nuovo controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Impostare i seguenti valori:

- Nome controller: **ProductController**
- Classe modello: **prodotto**
- Classe contesto dati: [selezionare **nuovo pulsante contesto dati** che compila i valori indicati di seguito]

![specificare il controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Fare clic su **Aggiungi** per creare il controller con codice generato automaticamente. Il codice include metodi per la creazione, il recupero, l'aggiornamento e l'eliminazione di istanze della classe Product. Il codice seguente illustra il metodo per aggiungere un prodotto. Si noti che il metodo restituisce un'istanza di **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult è una delle nuove funzionalità dell'API Web 2 e semplifica lo sviluppo di unit test.

Nella sezione successiva verrà personalizzato il codice generato per facilitare il passaggio di oggetti di test al controller.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Aggiungi inserimento delle dipendenze

Attualmente, la classe ProductController è hardcoded per l'uso di un'istanza della classe StoreAppContext. Si userà un modello denominato inserimento delle dipendenze per modificare l'applicazione e rimuovere la dipendenza hardcoded. Suddividendo questa dipendenza, è possibile passare un oggetto fittizio durante il test.

Fare clic con il pulsante destro del mouse sulla cartella **Models** e aggiungere una nuova interfaccia denominata **IStoreAppContext**.

Sostituire il codice con il seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Aprire il file StoreAppContext.cs e apportare le modifiche evidenziate di seguito. Le modifiche importanti da tenere presenti sono:

- La classe StoreAppContext implementa l'interfaccia IStoreAppContext
- Il metodo MarkAsModified è implementato

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Aprire il file ProductController.cs. Modificare il codice esistente in modo che corrisponda al codice evidenziato. Queste modifiche interrompono la dipendenza da StoreAppContext e consentono ad altre classi di passare un oggetto diverso per la classe Context. Questa modifica consente di passare un contesto di test durante gli unit test.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

È necessario apportare un'altra modifica in ProductController. Nel metodo **PutProduct** sostituire la riga che imposta lo stato dell'entità su Modified con una chiamata al metodo MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Compilare la soluzione.

A questo punto è possibile configurare il progetto di test.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installare i pacchetti NuGet nel progetto di test

Quando si usa il modello vuoto per creare un'applicazione, il progetto unit test (StoreApp. tests) non include i pacchetti NuGet installati. Altri modelli, ad esempio il modello API Web, includono alcuni pacchetti NuGet nel progetto unit test. Per questa esercitazione, è necessario includere il pacchetto di Entity Framework e il pacchetto Microsoft ASP.NET Web API 2 core nel progetto di test.

Fare clic con il pulsante destro del mouse sul progetto StoreApp. tests e scegliere **Gestisci pacchetti NuGet**. Per aggiungere i pacchetti al progetto, è necessario selezionare il progetto StoreApp. tests.

![Gestisci pacchetti](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Dai pacchetti online trovare e installare il pacchetto EntityFramework (versione 6,0 o successiva). Se sembra che il pacchetto EntityFramework sia già installato, è possibile che sia stato selezionato il progetto StoreApp anziché il progetto StoreApp. tests.

![Aggiungi Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Trovare e installare Microsoft ASP.NET pacchetto Web API 2 Core.

![installare il pacchetto di base dell'API Web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Chiudere la finestra Gestisci pacchetti NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Crea contesto di test

Aggiungere una classe denominata **TestDbSet** al progetto di test. Questa classe funge da classe di base per il set di dati di test. Sostituire il codice con il seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Aggiungere una classe denominata **TestProductDbSet** al progetto di test contenente il codice seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Aggiungere una classe denominata **TestStoreAppContext** e sostituire il codice esistente con il codice seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Creare test

Per impostazione predefinita, il progetto di test include un file di test vuoto denominato **UnitTest1.cs**. Questo file Mostra gli attributi usati per creare i metodi di test. Per questa esercitazione, è possibile eliminare questo file perché verrà aggiunta una nuova classe di test.

Aggiungere una classe denominata **TestProductController** al progetto di test. Sostituire il codice con il seguente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Esegui test

A questo punto è possibile eseguire i test. Verranno testati tutti i metodi contrassegnati con l'attributo **TestMethod** . Dalla voce di menu **test** eseguire i test.

![eseguire test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Aprire la finestra **Esplora test** e osservare i risultati dei test.

![risultati test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
