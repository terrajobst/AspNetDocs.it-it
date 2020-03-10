---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introduzione con OWIN e Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584672"
---
# <a name="getting-started-with-owin-and-katana"></a>Introduzione a OWIN e Katana

di [Mike Wasson](https://github.com/MikeWasson)

[Open Web Interface for .NET (OWIN)](http://owin.org/) definisce un'astrazione tra i server Web .NET e le applicazioni Web. Disaccoppiando il server Web dall'applicazione, OWIN rende più semplice la creazione di middleware per lo sviluppo Web .NET. OWIN semplifica inoltre la portabilità delle applicazioni Web in altri host&#8212;, ad esempio l'hosting self-service in un servizio di Windows o in un altro processo.

OWIN è una specifica di proprietà della community, non un'implementazione. Il progetto Katana è un set di componenti OWIN open source sviluppati da Microsoft. Per una panoramica generale di OWIN e Katana, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md). In questo articolo si passerà direttamente al codice per iniziare.

Questa esercitazione USA [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ma è anche possibile usare Visual Studio 2012. Alcuni passaggi sono diversi in Visual Studio 2012, come indicato di seguito.

## <a name="host-owin-in-iis"></a>OWIN host in IIS

In questa sezione si ospiterà OWIN in IIS. Questa opzione offre la flessibilità e la possibilità di composizione di una pipeline OWIN insieme al set di funzionalità di IIS più maturo. Utilizzando questa opzione, l'applicazione OWIN viene eseguita nella pipeline delle richieste ASP.NET.

Per prima cosa, creare un nuovo progetto di applicazione Web ASP.NET. (In Visual Studio 2012, utilizzare il tipo di progetto di applicazione Web ASP.NET vuoto).

![](getting-started-with-owin-and-katana/_static/image1.png)

Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare il modello **vuoto** .

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Aggiungere pacchetti NuGet

Aggiungere quindi i pacchetti NuGet necessari. Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**. Nella finestra console di gestione pacchetti digitare il comando seguente:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Aggiungere una classe Startup

Successivamente, aggiungere una classe di avvio OWIN. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, quindi selezionare **nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **Owin Startup Class**. Per altre informazioni sulla configurazione della classe startup, vedere [rilevamento della classe di avvio OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Aggiungere al metodo `Startup1.Configuration` il codice seguente:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Questo codice aggiunge una semplice porzione di middleware alla pipeline OWIN, implementata come funzione che riceve un'istanza **Microsoft. OWIN. IOwinContext** . Quando il server riceve una richiesta HTTP, la pipeline OWIN richiama il middleware. Il middleware imposta il tipo di contenuto per la risposta e scrive il corpo della risposta.

> [!NOTE]
> Il modello della classe di avvio OWIN è disponibile in Visual Studio 2013. Se si usa Visual Studio 2012, è sufficiente aggiungere una nuova classe vuota denominata `Startup1`e incollare il codice seguente:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Esecuzione dell'applicazione

Premere F5 per avviare il debug. In Visual Studio viene aperta una finestra del browser per `http://localhost:*port*/`. La pagina dovrebbe essere simile alla seguente:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>OWIN Self-host in un'applicazione console

È facile convertire questa applicazione dall'hosting di IIS al self-hosting in un processo personalizzato. Con l'hosting di IIS, IIS funge sia da server HTTP che dal processo che ospita il servizio. Con l'hosting automatico, l'applicazione crea il processo e usa la classe **HttpListener** come server http.

In Visual Studio creare una nuova applicazione console. Nella finestra console di gestione pacchetti digitare il comando seguente:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Aggiungere una classe `Startup1` dalla parte 1 di questa esercitazione al progetto. Non è necessario modificare questa classe.

Implementare il metodo di `Main` dell'applicazione come segue.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Quando si esegue l'applicazione console, il server inizia l'ascolto della `http://localhost:9000`. Se si passa a questo indirizzo in un Web browser, verrà visualizzata la pagina "Hello World".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Aggiungi diagnostica OWIN

Il pacchetto Microsoft. Owin. Diagnostics contiene il middleware che rileva le eccezioni non gestite e visualizza una pagina HTML con i dettagli dell'errore. Questa pagina funziona in modo molto simile alla pagina di errore ASP.NET, a volte detta "[schermata gialla della morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Analogamente a YSOD, la pagina di errore Katana è utile durante lo sviluppo, ma è consigliabile disabilitarla in modalità di produzione.

Per installare il pacchetto di diagnostica nel progetto, digitare il comando seguente nella finestra della console di gestione pacchetti:

`install-package Microsoft.Owin.Diagnostics –Pre`

Modificare il codice nel metodo di `Startup1.Configuration` come indicato di seguito:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Utilizzare ora CTRL + F5 per eseguire l'applicazione senza eseguire il debug, in modo che Visual Studio non si interrompa in caso di eccezione. L'applicazione si comporta come prima, fino a quando non si passa a `http://localhost/fail`, a quel punto l'applicazione genera l'eccezione. Il middleware della pagina di errore rileverà l'eccezione e visualizzerà una pagina HTML con informazioni sull'errore. È possibile fare clic sulle schede per visualizzare lo stack, la stringa di query, i cookie, l'intestazione della richiesta e le variabili di ambiente OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Passaggi successivi

- [Rilevamento della classe di avvio OWIN](owin-startup-class-detection.md)
- [Usare OWIN per ospitare autonomamente API Web ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Usare OWIN per l'hosting automatico di SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
