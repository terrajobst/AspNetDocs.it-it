---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Test della densità della connessione SignalR con Crank | Microsoft Docs
author: bradygaster
description: Test della densità di connessione di SignalR con Crank
ms.author: bradyg
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 901e039fbb81651ed18d560c99745b7e7f716e01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558338"
---
# <a name="signalr-connection-density-testing-with-crank"></a>Test della densità di connessione di SignalR con Crank

di [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo articolo descrive come usare lo strumento Crank per testare un'applicazione con più client simulati.

Quando l'applicazione è in esecuzione nell'ambiente host (un ruolo Web di Azure, IIS o self-hosted usando Owin), è possibile testare la risposta dell'applicazione a un livello elevato di densità di connessione usando lo strumento Crank. L'ambiente host può essere un server Internet Information Services (IIS), un host Owin o un ruolo Web di Azure. Nota: i contatori delle prestazioni non sono disponibili nelle app Web del servizio app Azure, quindi non sarà possibile ottenere dati sulle prestazioni da un test di densità della connessione.

La densità della connessione si riferisce al numero di connessioni TCP simultanee che è possibile stabilire in un server. Ogni connessione TCP comporta un sovraccarico e l'apertura di un numero elevato di connessioni inattive creerà infine un collo di bottiglia della memoria.

[La codebase di SignalR](https://github.com/signalr/signalr) include uno strumento di test di carico denominato **Crank**. La versione più recente di Crank si trova nel [ramo Dev](https://github.com/SignalR/signalr/tree/dev) su GitHub. È possibile scaricare un archivio zip del ramo Dev della codebase di SignalR [qui](https://github.com/SignalR/SignalR/archive/dev.zip).

La manovella può essere utilizzata per saturare completamente la memoria del server per calcolare il numero totale di connessioni inattive possibili nell'hardware del server. In alternativa, è anche possibile usare la manovella per eseguire il test di carico del server in una certa quantità di richieste di memoria, aumentando le connessioni fino a raggiungere un numero specifico o una soglia di memoria specifica.

Quando si esegue il test, è importante usare i client remoti per evitare qualsiasi competizione per le risorse (ad esempio, connessioni TCP e memoria). Monitorare i client per assicurarsi che non raggiungano colli di bottiglia che potrebbero impedire al server di raggiungere la capacità completa (memoria o CPU). Potrebbe essere necessario aumentare il numero di client per caricare completamente il server.

### <a name="running-a-connection-density-test"></a>Esecuzione di un test di densità della connessione

In questa sezione vengono descritti i passaggi necessari per eseguire un test della densità di connessione in un'applicazione SignalR.

1. Scaricare e compilare il [ramo Dev della codebase di SignalR](https://github.com/SignalR/SignalR/archive/dev.zip). Al prompt dei comandi passare alla directory &lt;progetto&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Distribuire l'applicazione nell'ambiente host previsto. Prendere nota dell'endpoint usato dall'applicazione. ad esempio, nell'applicazione creata nell' [esercitazione Introduzione](../getting-started/tutorial-getting-started-with-signalr.md), l'endpoint è `http://<yourhost>:8080/signalr`.
3. Installare i [contatori delle prestazioni di SignalR](signalr-performance.md#perfcounters) nel server. Se l'applicazione è in esecuzione in Azure, vedere [uso dei contatori delle prestazioni di SignalR in un ruolo Web di Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Dopo aver scaricato e compilato la codebase e installato i contatori delle prestazioni nell'host, lo strumento da riga di comando Crank si trova nella cartella `src\Microsoft.AspNet.SignalR.Crank\bin\Debug`.

Le opzioni disponibili per lo strumento Crank includono:

- **/?** : Mostra la schermata della guida. Se il parametro **URL** viene omesso, vengono visualizzate anche le opzioni disponibili.
- **/URL**: URL per le connessioni SignalR. Questo parametro è obbligatorio. Per un'applicazione SignalR che usa il mapping predefinito, il percorso termina con "/SignalR".
- **/Transport Level**: nome del trasporto usato. Il valore predefinito è `auto`, che consente di selezionare il protocollo più disponibile. Le opzioni includono `WebSockets`, `ServerSentEvents`e `LongPolling` (`ForeverFrame` non è un'opzione per Crank, perché viene usato il client .NET anziché Internet Explorer). Per ulteriori informazioni sul modo in cui SignalR seleziona i trasporti, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).
- **/BatchSize**: numero di client aggiunti in ogni batch. Il valore predefinito è 50.
- **/ConnectInterval**: intervallo in millisecondi tra l'aggiunta di connessioni. Il valore predefinito è 500.
- **/Connections**: numero di connessioni usate per il test di carico dell'applicazione. Il valore predefinito è 100.000.
- **/ConnectTimeout**: timeout in secondi prima dell'interruzione del test. Il valore predefinito è 300.
- **MinServerMBytes**: numero minimo di megabyte del server da raggiungere. Il valore predefinito è 500.
- **SendBytes**: dimensione del payload inviato al server in byte. Il valore predefinito è 0.
- **SendInterval**: ritardo in millisecondi tra messaggi al server. Il valore predefinito è 500.
- **SendTimeout nell'elemento**: timeout in millisecondi per i messaggi al server. Il valore predefinito è 300.
- **ControllerUrl**: URL in cui un client ospiterà un hub controller. Il valore predefinito è null (nessun hub controller). L'hub controller viene avviato all'avvio della sessione Crank; non viene effettuato alcun ulteriore contatto tra l'hub del controller e la manovella.
- **NumClients**: numero di client simulati per la connessione all'applicazione. Il valore predefinito è uno.
- **Logfile**: nome file del file di log per l'esecuzione dei test. Il valore predefinito è `crank.csv`.
- **SampleInterval**: tempo in millisecondi tra i campioni del contatore delle prestazioni. Il valore predefinito è 1000.
- **SignalRInstance**: nome dell'istanza per i contatori delle prestazioni nel server. Per impostazione predefinita, viene utilizzato lo stato della connessione client.

### <a name="example"></a>Esempio

Il seguente comando eseguirà il test di un sito denominato `pfsignalr` in Azure che ospita un'applicazione sulla porta 8080 con un Hub denominato "ControllerHub", usando le connessioni 100.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
