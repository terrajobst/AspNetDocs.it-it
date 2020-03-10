---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-host API Web ASP.NET 1 (C#)-ASP.NET 4. x
author: MikeWasson
description: Esercitazione con codice Mostra come ospitare un'API Web all'interno di un'applicazione console.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525088"
---
# <a name="self-host-aspnet-web-api-1-c"></a>Self-host API Web ASP.NET 1 (C#)

di [Mike Wasson](https://github.com/MikeWasson)

> Questa esercitazione illustra come ospitare un'API Web all'interno di un'applicazione console. API Web ASP.NET non richiede IIS. È possibile ospitare autonomamente un'API Web nel proprio processo host. 
> 
> **Le nuove applicazioni devono usare OWIN per l'hosting self-service dell'API Web.** Vedere [usare OWIN per ospitare autonomamente API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - API Web 1
> - Visual Studio 2012

## <a name="create-the-console-application-project"></a>Creare il progetto di applicazione console

Avviare Visual Studio e selezionare **nuovo progetto** nella pagina **iniziale** . In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** . In **Visual C#** Selezionare **Windows**. Nell'elenco dei modelli di progetto selezionare **applicazione console**. Denominare il progetto &quot;SelfHost&quot; e fare clic su **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Impostare il Framework di destinazione (Visual Studio 2010)

Se si usa Visual Studio 2010, modificare il Framework di destinazione in .NET Framework 4,0. Per impostazione predefinita, il modello di progetto è destinato a [.NET Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**. Nell'elenco a discesa **Framework di destinazione** impostare Framework di destinazione su .NET Framework 4,0. Quando viene richiesto di applicare la modifica, fare clic su **Sì**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Installare Gestione pacchetti NuGet

Gestione pacchetti NuGet è il modo più semplice per aggiungere gli assembly dell'API Web a un progetto non-ASP.NET.

Per verificare se Gestione pacchetti NuGet è installato, fare clic sul menu **strumenti** in Visual Studio. Se viene visualizzata una voce di menu denominata **Gestione pacchetti NuGet**, si avrà gestione pacchetti NuGet.

Per installare Gestione pacchetti NuGet:

1. Avviare Visual Studio.
2. Scegliere **Estensioni e aggiornamenti** dal menu **Strumenti**.
3. Nella finestra di dialogo **estensioni e aggiornamenti** selezionare **online**.
4. Se non viene visualizzato "Gestione pacchetti NuGet", digitare "Gestione pacchetti NuGet" nella casella di ricerca.
5. Selezionare Gestione pacchetti NuGet e fare clic su **download**.
6. Al termine del download, verrà richiesto di installare.
7. Al termine dell'installazione, potrebbe essere richiesto di riavviare Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Aggiungere il pacchetto NuGet dell'API Web

Dopo l'installazione di gestione pacchetti NuGet, aggiungere il pacchetto Self-host dell'API Web al progetto.

1. Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**. *Nota*: se questa voce di menu non è visibile, assicurarsi che Gestione pacchetti NuGet sia installato correttamente.
2. Selezionare **Gestisci pacchetti NuGet per la soluzione**
3. Nella finestra di dialogo **Gestisci pacchetti NugGet** selezionare **online**.
4. Nella casella di ricerca digitare &quot;Microsoft. AspNet. WebApi. SelfHost&quot;.
5. Selezionare il pacchetto Self-host API Web ASP.NET e fare clic su **Installa**.
6. Al termine dell'installazione del pacchetto, fare clic su **Chiudi** per chiudere la finestra di dialogo.

> [!NOTE]
> Assicurarsi di installare il pacchetto denominato Microsoft. AspNet. WebApi. SelfHost, non AspNetWebApi. SelfHost.

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Creare il modello e il controller

Questa esercitazione usa le stesse classi di modello e controller dell'esercitazione [Introduzione](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .

Aggiungere una classe pubblica denominata `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Aggiungere una classe pubblica denominata `ProductsController`. Derivare questa classe da **System. Web. http. ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Per ulteriori informazioni sul codice in questo controller, vedere l'esercitazione [Introduzione](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) . Questo controller definisce tre azioni GET:

| URI | Description |
| --- | --- |
| /api/products | Ottenere un elenco di tutti i prodotti. |
| *ID* /API/Products/ | Ottenere un prodotto in base all'ID. |
| /API/Products/? Category =*categoria* | Ottenere un elenco di prodotti in base alla categoria. |

## <a name="host-the-web-api"></a>Ospitare l'API Web

Aprire il file Program.cs e aggiungere le istruzioni using seguenti:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Aggiungere il codice seguente alla classe **Program** .

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>Opzionale Aggiungere una prenotazione dello spazio dei nomi URL HTTP

Questa applicazione è in ascolto del `http://localhost:8080/`. Per impostazione predefinita, l'ascolto di un particolare indirizzo HTTP richiede privilegi di amministratore. Quando si esegue l'esercitazione, è possibile che venga ricevuto questo errore: "HTTP non è stato in grado di registrare l'URL http://+:8080/" esistono due modi per evitare questo errore:

- Eseguire Visual Studio con autorizzazioni di amministratore con privilegi elevati o
- Usare netsh. exe per concedere all'account le autorizzazioni per riservare l'URL.

Per usare netsh. exe, aprire un prompt dei comandi con privilegi di amministratore e immettere il comando seguente:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

dove *computer\nome utente* è l'account utente.

Al termine dell'hosting automatico, assicurarsi di eliminare la prenotazione:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Chiamare l'API Web da un'applicazione client (C#)

Scriviamo una semplice applicazione console che chiama l'API Web.

Aggiungere un nuovo progetto di applicazione console alla soluzione:

- In Esplora soluzioni fare clic con il pulsante destro del mouse sulla soluzione e scegliere **Aggiungi nuovo progetto**.
- Creare una nuova applicazione console denominata &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Usare Gestione pacchetti NuGet per aggiungere il pacchetto delle librerie di API Web ASP.NET Core:

- Dal menu Strumenti selezionare **Gestione pacchetti NuGet**.
- Selezionare **Gestisci pacchetti NuGet per la soluzione**
- Nella finestra di dialogo **Gestisci pacchetti NuGet** selezionare **online**.
- Nella casella di ricerca digitare &quot;Microsoft. AspNet. WebApi. client&quot;.
- Selezionare il pacchetto delle librerie client dell'API Web Microsoft ASP.NET e fare clic su **Installa**.

Aggiungere un riferimento in ClientApp al progetto SelfHost:

- In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto ClientApp.
- Selezionare **Aggiungi riferimento**.
- Nella finestra di dialogo **Gestione riferimenti** selezionare **progetti**in **soluzione**.
- Selezionare il progetto SelfHost.
- Fare clic su **OK**.

![](self-host-a-web-api/_static/image6.png)

Aprire il file client/program. cs. Aggiungere l'istruzione **using** seguente:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Aggiungere un'istanza di **HttpClient** statica:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Aggiungere i metodi seguenti per elencare tutti i prodotti, elencare un prodotto in base all'ID ed elencare i prodotti in base alla categoria.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Ognuno di questi metodi segue lo stesso modello:

1. Chiamare **HttpClient. GetAsync** per inviare una richiesta GET all'URI appropriato.
2. Chiamare **HttpResponseMessage. EnsureSuccessStatusCode**. Questo metodo genera un'eccezione se lo stato della risposta HTTP è un codice di errore.
3. Chiamare **ReadAsAsync&lt;t&gt;** per deserializzare un tipo CLR dalla risposta http. Questo metodo è un metodo di estensione definito in **System .NET. http. HttpContentExtensions**.

I metodi **GetAsync** e **ReadAsAsync** sono entrambi asincroni. Restituiscono oggetti **Task** che rappresentano l'operazione asincrona. Il recupero della proprietà **result** blocca il thread fino al completamento dell'operazione.

Per altre informazioni sull'uso di HttpClient, incluso come eseguire chiamate non bloccanti, vedere [chiamata di un'API Web da un client .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Prima di chiamare questi metodi, impostare la proprietà BaseAddress nell'istanza di HttpClient su "`http://localhost:8080`". Esempio:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Questa operazione dovrebbe restituire gli elementi seguenti. Ricordarsi di eseguire prima l'applicazione SelfHost.

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
