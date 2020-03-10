---
uid: web-api/samples-list
title: Elenco di esempi di API Web-ASP.NET 4. x
author: rick-anderson
description: API Web ASP.NET elenco di esempi per ASP.NET 4. x
ms.author: riande
ms.date: 09/18/2012
ms.custom: seoapril2019
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 24368fc80e7fc3c840f1f1f9accf13fa15a06c1b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598462"
---
# <a name="web-api-samples-list"></a>Elenco di esempi di API Web

## <a name="httpclient-samples"></a>Esempi di HttpClient

**Esempio di esempio di traduzione Bing** | [vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/BingTranslateSample)

Viene illustrato come chiamare il [servizio Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) utilizzando la classe **HttpClient** . L'API del servizio Microsoft Translator richiede un token OAuth, che l'applicazione ottiene inviando una richiesta al server token di Azure per ogni richiesta al servizio di conversione. Il risultato del server di token viene inserito nella richiesta inviata al servizio di traduzione. Prima di eseguire questo esempio, è necessario ottenere una [chiave dell'applicazione da Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) e inserire le informazioni nella classe di esempio AccessTokenMessageHandler.

**Esempio di Google Maps** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [origine di Visual Studio 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/GoogleMapsSample)

USA **HttpClient** per scaricare una mappa di Redmond, WA dall' [API di Google Maps](https://developers.google.com/maps/), la Salva come file locale e apre il Visualizzatore immagini predefinito.

**Esempio di client Twitter** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [origine di Visual Studio 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/TwitterSample)

Viene illustrato come scrivere un semplice client Twitter usando **HttpClient**. Nell'esempio viene usato un **HttpMessageHandler** per inserire le informazioni di autenticazione OAuth nel **HttpRequestMessage**in uscita. Il risultato di Twitter viene letto usando JSON.NET. Prima di eseguire questo esempio, è necessario ottenere una [chiave dell'applicazione da Twitter](https://dev.twitter.com/)e inserire le informazioni nella classe di esempio OAuthMessageHandler.

**Esempio della Banca mondiale** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [origine di visual Studio 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net40) | [vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net45)

Viene illustrato come recuperare dati dal sito di dati della Banca mondiale usando JSON.NET per analizzare il risultato.

## <a name="web-api-samples"></a>Esempi di API Web

**Introduzione con API Web ASP.NET** [origine | vs 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Viene illustrato come creare un'API Web di base che supporta le richieste HTTP GET. Contiene il codice sorgente per l'esercitazione il [primo API Web ASP.NET](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Scenari di API Web ASP.NET JavaScript-commenti** | [origine vs 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Illustra come usare API Web ASP.NET per creare API Web che supportano i client del browser e che possono essere facilmente chiamate tramite jQuery.

Origine **Contact Manager** | [vs 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Questo esempio usa API Web ASP.NET per creare una semplice applicazione Contact Manager. L'applicazione è costituita da un'API Web Contact Manager usata da un'applicazione MVC ASP.NET e da un'applicazione Windows Phone per visualizzare e gestire un elenco di contatti.

**Esempio di batch** | [Descrizione dettagliata](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [origine di Visual Studio 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/BatchSample)

Viene illustrato come implementare l'invio in batch HTTP all'interno di ASP.NET. L'invio in batch consiste nell'inserire più richieste HTTP all'interno di un singolo corpo di entità multipart MIME, che viene quindi inviato al server come HTTP POST. Le richieste vengono elaborate singolarmente e le risposte vengono inserite in un altro corpo di entità MIME multipart, che viene restituito al client.

**Esempio di controller di contenuto** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [origine di visual Studio 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net40) | [vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net45)

Viene illustrato come leggere e scrivere entità di richiesta e risposta in modo asincrono utilizzando i flussi. Il controller di esempio è costituito da due azioni: un'azione PUT che legge il corpo dell'entità della richiesta in modo asincrono e la archivia in un file locale e un'azione GET che restituisce il contenuto del file locale.

**Esempio di resolver di assembly personalizzato** | [origine vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomAssemblyResolverSample)

Viene illustrato come modificare API Web ASP.NET per supportare l'individuazione di controller da un assembly di libreria caricato dinamicamente. Nell'esempio viene implementato un **IAssembliesResolver** personalizzato che chiama l'implementazione predefinita e quindi aggiunge l'assembly di libreria ai risultati predefiniti.

**Esempio di formattatore di media type personalizzato** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [origine vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomMediaTypeFormatterSample)

Viene illustrato come creare un formattatore di media type personalizzato utilizzando la classe di base **BufferedMediaTypeFormatter** . Questa classe di base è destinata ai formattatori che utilizzano principalmente operazioni di lettura e scrittura sincrone. Oltre a visualizzare il formattatore di Media Type, nell'esempio viene illustrato come associarlo registrando l'applicazione come parte di **HttpConfiguration** per l'applicazione. Si noti che è anche possibile usare direttamente la classe di base **MediaTypeFormatter** per i formattatori che usano principalmente operazioni di lettura e scrittura asincrone.

**Esempio di associazione di parametri personalizzati** | [Descrizione dettagliata](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [origine di Visual Studio 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomParameterBinding)

Viene illustrato come personalizzare il processo di associazione dei parametri, ovvero il processo che determina il modo in cui le informazioni provenienti da una richiesta sono associate ai parametri di azione. In questo esempio, il controller Home è costituito da quattro azioni:

1. BindPrincipal Mostra come associare un parametro IPrincipal da un'entità generica personalizzata, non da un messaggio HTTP GET;
2. BindCustomComplexTypeFromUriOrBody Mostra come associare un parametro di tipo complesso, che può essere dal corpo del messaggio o dall'URI della richiesta di un messaggio HTTP POST;
3. BindCustomComplexTypeFromUriWithRenamedProperty Mostra come associare un parametro di tipo complesso a una proprietà rinominata che deriva dall'URI della richiesta di un messaggio HTTP POST;
4. PostMultipleParametersFromBody Mostra come associare più parametri dal corpo per un messaggio POST;

**Esempio di caricamento di File** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [origine di Visual Studio 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/FileUploadSample)

Mostra come caricare file in un **ApiController** usando il caricamento di file multipart MIME e come configurare le notifiche di stato con **HttpClient** usando **ProgressNotificationHandler**. Il controller legge il contenuto di un file HTML caricato in modo asincrono e scrive una o più parti del corpo in un file locale. La risposta contiene informazioni sul file o i file caricati.

**Esempio di caricamento di file nell'archivio BLOB di Azure** | [Descrizione dettagliata](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [origine di Visual Studio 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/AzureBlobsFileUploadSample)

Questo esempio è simile all'esempio di caricamento dei file, ma invece di salvare i file caricati sul disco locale, i file vengono caricati in modo asincrono nell' [Archivio BLOB di Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) tramite [Windows Azure SDK per .NET](https://www.windowsazure.com/develop/net/). Fornisce anche un meccanismo per elencare i BLOB attualmente presenti in un [contenitore di archiviazione BLOB di Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). È possibile provare l'esempio in esecuzione nell' **emulatore di archiviazione di Azure** incluso in Azure SDK. Se si ha un [account di archiviazione di Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), è possibile eseguire anche sul servizio di archiviazione reale.

**Esempio di pipeline di gestione messaggi Http** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [origine vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/HttpMessageHandlerPipelineSample)

Mostra come collegare le istanze di **HttpMessageHandler** sia nel client (**HttpClient**) che nel server (API Web ASP.NET). Nell'esempio, lo stesso gestore viene usato sia sul client che sul server. Sebbene sia raro che lo stesso gestore venga eseguito in entrambe le posizioni, il modello a oggetti è lo stesso sul lato client e server.

**Esempio di caricamento JSON** | [vs 2012 source](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/JsonUploadSample)

Viene illustrato come caricare e scaricare JSON da e verso un **ApiController**. Nell'esempio viene utilizzata una **ApiController** minima e viene eseguita l'accesso tramite **HttpClient**.

**Esempio di Mashup** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [origine di Visual Studio 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MashupSample)

Viene illustrato come accedere a più siti remoti in modo asincrono dall'interno di un'azione **ApiController** . Ogni volta che viene raggiunta l'azione, le richieste vengono eseguite in modo asincrono, in modo che nessun thread venga bloccato.

**Esempio di traccia della memoria** | [Descrizione dettagliata](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [origine di Visual Studio 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MemoryTracingSample)

Questo progetto di esempio crea un pacchetto NuGet che installerà un writer di traccia in memoria personalizzato in API Web ASP.NET applicazioni.

**Esempio di MongoDB** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [origine di Visual Studio 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MongoSample)

Viene illustrato come usare MongoDB come archivio permanente per un **ApiController**, usando un modello di repository.

**Esempio di processore del corpo della risposta** | [vs 2012 origine](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ResponseEntityProcessorSample)

Viene illustrato come copiare un'entità di risposta, ovvero un corpo della risposta HTTP, in un file locale prima che questo venga trasmesso al client ed eseguire l'elaborazione aggiuntiva su tale file in modo asincrono. Nell'esempio viene implementato un **HttpMessageHandler** che esegue il wrapping dell'entità di risposta con una che scrive entrambi nell'output come di consueto e in un file locale.

**Caricare l'esempio XDocument** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [origine di Visual Studio 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/UploadXDocumentSample)

Mostra come caricare un oggetto XDocument in un **ApiController** usando **PushStreamContent** e **HttpClient**.

**Esempio di convalida** | [origine vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ValidationSample)

Mostra come è possibile usare gli attributi di convalida nei modelli in ASP.NET WebAPI per convalidare il contenuto della richiesta HTTP. Viene illustrato come contrassegnare le proprietà come richieste, come utilizzare gli attributi di convalida personalizzati e definiti dal Framework per annotare il modello e come restituire risposte di errore per gli Stati del modello non validi.

**Esempio di Web Form** | [Descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [origine vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/WebFormSample)

Mostra un ApiController aggiunto a un progetto Web Form.

**[Esempio RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs è una semplice applicazione di rilevamento dei bug che Mostra come usare API Web ASP.NET e la nuova libreria client HTTP per creare un sistema basato su ipermedia. L'esempio include le implementazioni di client e server, usando API Web ASP.NET. Il server usa un formattatore Razor personalizzato per generare rappresentazioni di risorse. L'esempio fornisce anche un server node. js per illustrare i vantaggi derivanti dall'uso di una progettazione ipermedia per separare client e server.
