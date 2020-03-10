---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Chiamata dell'API Web da un'applicazione Windows Phone 8C#()-ASP.NET 4. x
author: rmcmurray
description: "Esercitazione con codice: creare un'applicazione API Web ASP.NET in ASP.NET 4. x che fornisce un catalogo di libri a un'applicazione Windows Phone 8."
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614737"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Chiamata dell'API Web da un'applicazione Windows Phone 8 (C#)

di [Robert McMurray](https://github.com/rmcmurray)

In questa esercitazione si apprenderà come creare uno scenario end-to-end completo costituito da un'applicazione API Web ASP.NET che fornisce un catalogo di libri a un'applicazione Windows Phone 8.

### <a name="overview"></a>Panoramica

I servizi RESTful come API Web ASP.NET semplificano la creazione di applicazioni basate su HTTP per gli sviluppatori astraendo l'architettura per le applicazioni lato server e lato client. Anziché creare un protocollo proprietario basato su socket per la comunicazione, gli sviluppatori di API Web devono semplicemente pubblicare i metodi HTTP necessari per la propria applicazione, ad esempio GET, POST, PUT, DELETE, e gli sviluppatori di applicazioni client devono solo utilizzare metodi HTTP necessari per l'applicazione.

In questa esercitazione end-to-end si apprenderà come usare l'API Web per creare i progetti seguenti:

- Nella [prima parte di questa esercitazione](#STEP1)si creerà un'applicazione API Web ASP.NET che supporta tutte le operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) per la gestione di un catalogo di libri. Questa applicazione utilizzerà il [file XML di esempio (Books. Xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) di MSDN.
- Nella [seconda parte di questa esercitazione](#STEP2)si creerà un'applicazione interactive Windows Phone 8 che recupera i dati dall'applicazione API Web.

#### <a name="prerequisites"></a>Prerequisiti

- Visual Studio 2013 con Windows Phone 8 SDK installato
- Windows 8 o versione successiva in un sistema a 64 bit con Hyper-V installato
- Per un elenco di requisiti aggiuntivi, vedere la sezione *requisiti di sistema* nella pagina di download di [Windows Phone SDK 8,0](https://www.microsoft.com/download/details.aspx?id=35471) .

> [!NOTE]
> Per eseguire il test della connettività tra l'API Web e Windows Phone 8 progetti nel sistema locale, è necessario seguire le istruzioni riportate nell'articolo *[connettere l'emulatore Windows Phone 8 all'API Web in un computer locale](https://go.microsoft.com/fwlink/?LinkId=324014)* per configurare l'ambiente di test.

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Passaggio 1: creazione del progetto Bookstore dell'API Web

Il primo passaggio di questa esercitazione end-to-end consiste nel creare un progetto API Web che supporta tutte le operazioni CRUD. Si noti che nel [passaggio 2](#STEP2) di questa esercitazione verrà aggiunto il progetto di applicazione Windows Phone alla soluzione.

1. Aprire **Visual Studio 2013**.
2. Fare clic su **file**, quindi su **nuovo**e infine su **progetto**.
3. Quando viene visualizzata la finestra di dialogo **nuovo progetto** , espandere **installato**, **modelli**, quindi **oggetto C#visivo** , quindi **Web**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Fare clic sull'immagine per espanderla                                                                |

4. Evidenziare **ASP.NET Web Application**, immettere **Bookstore** per il nome del progetto e quindi fare clic su **OK**.
5. Quando viene visualizzata la finestra di dialogo **nuovo progetto ASP.NET** , selezionare il modello **API Web** e quindi fare clic su **OK**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Fare clic sull'immagine per espanderla                                                                |

6. Quando si apre il progetto API Web, rimuovere il controller di esempio dal progetto:

    1. Espandere la cartella **controller** in Esplora soluzioni.
    2. Fare clic con il pulsante destro del mouse sul file **ValuesController.cs** e scegliere **Elimina**.
    3. Quando viene richiesto di confermare l'eliminazione, fare clic su **OK** .
7. Aggiungere un file di dati XML al progetto API Web; Questo file contiene il contenuto del catalogo della libreria:

   1. Fare clic con il pulsante destro del mouse sulla cartella **App\_data** in Esplora soluzioni, scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.
   2. Quando viene visualizzata la finestra di dialogo **Aggiungi nuovo elemento** , evidenziare il modello di **file XML** .
   3. Denominare il file **books. XML**e quindi fare clic su **Aggiungi**.
   4. Quando si apre il file **books. XML** , sostituire il codice nel file con il codice XML del file **books. XML** di esempio su MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Salvare e chiudere il file XML.

8. Aggiungere il modello Bookstore al progetto API Web; Questo modello contiene la logica di creazione, lettura, aggiornamento ed eliminazione (CRUD) per l'applicazione Bookstore:

   1. Fare clic con il pulsante destro del mouse sulla cartella **Models** in Esplora soluzioni, scegliere **Aggiungi**e quindi fare clic su **classe**.
   2. Quando viene visualizzata la finestra di dialogo **Aggiungi nuovo elemento** , denominare il file di classe **bookdetails.cs**, quindi fare clic su **Aggiungi**.
   3. Quando viene aperto il file **bookdetails.cs** , sostituire il codice nel file con il codice seguente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Salvare e chiudere il file **bookdetails.cs** .

9. Aggiungere il controller Bookstore al progetto API Web:

   1. Fare clic con il pulsante destro del mouse sulla cartella **controller** in Esplora soluzioni, scegliere **Aggiungi**e quindi fare clic su **controller**.
   2. Quando viene visualizzata la finestra di dialogo **Aggiungi impalcatura** , evidenziare **Controller Web API 2-vuoto**e quindi fare clic su **Aggiungi**.
   3. Quando viene visualizzata la finestra di dialogo **Aggiungi controller** , denominare il controller **BooksController**, quindi fare clic su **Aggiungi**.
   4. Quando viene aperto il file **BooksController.cs** , sostituire il codice nel file con il codice seguente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Salvare e chiudere il file **BooksController.cs** .

10. Compilare l'applicazione API Web per verificare la presenza di errori.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Passaggio 2: aggiunta del progetto di catalogo della libreria Windows Phone 8

Il passaggio successivo di questo scenario end-to-end consiste nel creare l'applicazione di catalogo per Windows Phone 8. Questa applicazione userà il modello di *app di Windows Phone Data Binding* per l'interfaccia utente predefinita e userà l'applicazione API Web creata nel [passaggio 1](#STEP1) di questa esercitazione come origine dati.

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione **Bookstore** , quindi scegliere **Aggiungi**, quindi **nuovo progetto**.
2. Quando viene visualizzata la finestra di dialogo **nuovo progetto** , espandere **installato**, quindi oggetto **visivo C#** , quindi **Windows Phone**.
3. Evidenziare **Windows Phone app con associazione**a file, immettere **BookCatalog** per nome e quindi fare clic su **OK**.
4. Aggiungere il pacchetto NuGet Json.NET al progetto **BookCatalog** :

    1. Fare clic con il pulsante destro del mouse su **riferimenti** per il progetto **BookCatalog** in Esplora soluzioni e quindi scegliere **Gestisci pacchetti NuGet**.
    2. Quando viene visualizzata la finestra di dialogo **Gestisci pacchetti NuGet** , espandere la sezione **Online** ed evidenziare **NuGet.org**.
    3. Immettere **JSON.NET** nel campo di ricerca e fare clic sull'icona di ricerca.
    4. Evidenziare **JSON.NET** nei risultati della ricerca e quindi fare clic su **Installa**.
    5. Al termine dell'installazione, fare clic su **Chiudi**.
5. Aggiungere il modello **BookDetails** al progetto **BookCatalog** . contiene un modello generico della classe Bookstore:

   1. Fare clic con il pulsante destro del mouse sul progetto **BookCatalog** in Esplora soluzioni, scegliere **Aggiungi**e quindi fare clic su **nuova cartella**.
   2. Assegnare un nome ai nuovi **modelli**di cartella.
   3. Fare clic con il pulsante destro del mouse sulla cartella **Models** in Esplora soluzioni, scegliere **Aggiungi**e quindi fare clic su **classe**.
   4. Quando viene visualizzata la finestra di dialogo **Aggiungi nuovo elemento** , denominare il file di classe **bookdetails.cs**, quindi fare clic su **Aggiungi**.
   5. Quando viene aperto il file **bookdetails.cs** , sostituire il codice nel file con il codice seguente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Salvare e chiudere il file **bookdetails.cs** .

6. Aggiornare la classe **MainViewModel.cs** per includere la funzionalità per comunicare con l'applicazione API Web della libreria:

   1. Espandere la cartella **ViewModels** in Esplora soluzioni, quindi fare doppio clic sul file **MainViewModel.cs** .
   2. Quando viene aperto il file **MainViewModel.cs** , sostituire il codice nel file con il codice seguente: Si noti che sarà necessario aggiornare il valore della costante `apiUrl` con l'URL effettivo dell'API Web: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Salvare e chiudere il file **MainViewModel.cs** .

7. Aggiornare il file **MainPage. XAML** per personalizzare il nome dell'applicazione:

   1. Fare doppio clic sul file **MainPage. XAML** in Esplora soluzioni.
   2. Quando si apre il file **MainPage. XAML** , individuare le righe di codice seguenti: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Sostituire le righe con le seguenti: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Salvare e chiudere il file **MainPage. XAML** .

8. Aggiornare il file **DetailsPage. XAML** per personalizzare gli elementi visualizzati:

   1. Fare doppio clic sul file **DetailsPage. XAML** in Esplora soluzioni.
   2. Quando viene aperto il file **DetailsPage. XAML** , individuare le righe di codice seguenti: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Sostituire le righe con le seguenti: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Salvare e chiudere il file **DetailsPage. XAML** .

9. Compilare l'applicazione Windows Phone per verificare la presenza di errori.

### <a name="step-3-testing-the-end-to-end-solution"></a>Passaggio 3: test della soluzione end-to-end

Come indicato nella sezione *prerequisiti* di questa esercitazione, quando si esegue il test della connettività tra l'API web e Windows Phone 8 progetti nel sistema locale, è necessario seguire le istruzioni riportate nell'articolo *[connettere l'emulatore Windows Phone 8 all'API Web in un computer locale](https://go.microsoft.com/fwlink/?LinkId=324014)* per configurare l'ambiente di testing.

Una volta configurato l'ambiente di testing, sarà necessario impostare l'applicazione Windows Phone come progetto di avvio. A tale scopo, evidenziare l'applicazione **BookCatalog** in Esplora soluzioni e quindi fare clic su **Imposta come progetto di avvio**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Fare clic sull'immagine per espanderla |

Quando si preme F5, Visual Studio avvierà sia l'emulatore di Windows Phone, che visualizzerà una &quot;attendere&quot; messaggio mentre i dati dell'applicazione vengono recuperati dall'API Web:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Fare clic sull'immagine per espanderla |

Se tutti gli elementi hanno esito positivo, verrà visualizzato il catalogo:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Fare clic sull'immagine per espanderla |

Se si tocca un titolo di libro, l'applicazione visualizzerà la descrizione del libro:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Fare clic sull'immagine per espanderla |

Se l'applicazione non è in grado di comunicare con l'API Web, verrà visualizzato un messaggio di errore:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Fare clic sull'immagine per espanderla |

Se si tocca il messaggio di errore, verranno visualizzati altri dettagli sull'errore:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Fare clic sull'immagine per espanderla                                                                 |
