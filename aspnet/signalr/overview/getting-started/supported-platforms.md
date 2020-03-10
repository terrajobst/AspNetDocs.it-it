---
uid: signalr/overview/getting-started/supported-platforms
title: Piattaforme supportate | Microsoft Docs
author: bradygaster
description: Questo articolo descrive quali client e server sono supportati da SignalR.
ms.author: bradyg
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 89730e591bb94022d16fe1a78a4350b38e0bf7a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558632"
---
# <a name="supported-platforms"></a>Piattaforme supportate

di [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Questo articolo descrive quali client e server sono supportati da SignalR. 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

SignalR è supportato in un'ampia gamma di configurazioni server e client. Ogni opzione di trasporto dispone inoltre di un set di requisiti specifici; Se i requisiti di sistema per un trasporto non sono disponibili, SignalR esegue correttamente il failover su altri trasporti. Per ulteriori informazioni sui trasporti supportati da SignalR, vedere [trasporti e fallback](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Requisiti di sistema del server di

Il componente server SignalR può essere ospitato in un'ampia gamma di configurazioni server. In questa sezione vengono descritte le versioni supportate dei sistemi operativi, .NET Framework, Internet Information Server e altri componenti.

### <a name="supported-server-operating-systems"></a>Sistemi operativi server supportati

Il componente server SignalR può essere ospitato nei seguenti sistemi operativi server o client. Si noti che per SignalR è necessario usare WebSocket, Windows Server 2012, Windows Server 2016 o Windows 8 (WebSocket può essere usato in siti Web di Microsoft Azure, purché la versione di .NET Framework del sito sia impostata su 4,5 e i socket Web siano abilitati nel Pagina Configurazione).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Microsoft Azure

### <a name="supported-server-net-framework-version"></a>Versione server .NET Framework supportata

SignalR 2 è supportato solo in .NET Framework 4,5. Vedere la sezione [aggiornamenti consigliati](#updates) per gli aggiornamenti che migliorano l'affidabilità, la compatibilità, la stabilità e le prestazioni.

### <a name="supported-server-iis-versions"></a>Versioni di IIS del server supportate

Quando SignalR è ospitato in IIS, sono supportate le versioni seguenti. Si noti che se si utilizza un sistema operativo client, ad esempio per lo sviluppo (Windows 8 o Windows 7), non è consigliabile utilizzare le versioni complete di IIS o Cassini, poiché sarà previsto un limite di 10 connessioni simultanee, che verranno raggiunte molto rapidamente dalle connessioni sono temporanei e ristabiliti e non vengono eliminati immediatamente quando non vengono più usati. IIS Express deve essere utilizzato nei sistemi operativi client.

Si noti inoltre che per SignalR è necessario usare WebSocket, è necessario usare IIS 8 o IIS 8 Express, il server deve usare Windows 8, Windows Server 2012 o versione successiva e WebSocket deve essere abilitato in IIS. Per informazioni su come abilitare WebSocket in IIS, vedere [supporto del protocollo WebSocket per iis 8,0](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 o IIS 8 Express.
- IIS 7 e 7,5. È necessario il supporto per gli [URL con estensione](https://support.microsoft.com/kb/980368) .
- IIS deve essere eseguito in modalità integrata. la modalità classica non è supportata. I ritardi dei messaggi fino a 30 secondi possono essere riscontrati se IIS viene eseguito in modalità classica utilizzando il trasporto degli eventi inviati dal server.
- L'applicazione host deve essere in esecuzione in modalità di attendibilità totale.

## <a name="client-system-requirements"></a>Requisiti di sistema del client di

SignalR può essere usato in un'ampia gamma di piattaforme client. In questa sezione vengono descritti i requisiti di sistema per l'utilizzo di SignalR in Web browser, applicazioni desktop di Windows, applicazioni Silverlight e dispositivi mobili.

### <a name="web-browsers"></a>Web browser

SignalR può essere usato in un'ampia gamma di browser Web, ma in genere sono supportate solo le due versioni più recenti.

Le applicazioni che usano SignalR nei browser devono usare la versione jQuery 1.6.4 o le versioni successive principali, ad esempio 1.7.2, 1.8.2 o 1.9.1.

SignalR può essere usato nei browser seguenti:

- Microsoft Internet Explorer versioni 8, 9, 10 e 11. Sono supportate le versioni moderne, desktop e per dispositivi mobili.
- Mozilla Firefox: versione corrente: 1, entrambe le versioni Windows e Mac.
- Google Chrome: versione corrente: 1, entrambe le versioni Windows e Mac.
- Safari: versione corrente: 1, sia le versioni Mac che iOS.
- Opera: versione corrente-1, solo Windows.
- Browser Android

Oltre a richiedere determinati browser, i vari trasporti usati da SignalR hanno i propri requisiti. I trasporti seguenti sono supportati nelle configurazioni seguenti:

<a id="browser"></a>

**Requisiti per il trasporto del browser Web**

| Trasporto | Internet Explorer | Chrome (Windows o iOS) | Firefox | Safari (OSX o iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| Oggetti WebSocket | 10+ | corrente-1 | corrente-1 | corrente-1 | N/D |
| Eventi inviati dal server | N/D | corrente-1 | corrente-1 | corrente-1 | N/D |
| ForeverFrame | 8+ | N/D | N/D | N/D | 4.1 |
| Polling prolungato | 8+ | corrente-1 | corrente-1 | corrente-1 | 4.1 |

\*: 6 + richiesto per la funzionalità completa.

#### <a name="unsupported-browsers"></a>Browser non supportati

Sebbene SignalR *possa* essere eseguito senza problemi principali nelle versioni precedenti del browser, il SignalR non viene attivamente testato e in genere non vengono risolti i bug che possono essere visualizzati in essi.

### <a name="windows-desktop-and-silverlight-applications"></a>Applicazioni desktop e Silverlight di Windows

Oltre a essere in esecuzione in un Web browser, SignalR può essere ospitato in applicazioni client Windows o Silverlight autonome. Le applicazioni Windows desktop e Silverlight SignalR presentano i requisiti di sistema seguenti.

- Le applicazioni che utilizzano .NET 4 sono supportate in Windows XP SP3 o versioni successive.
- Le applicazioni che utilizzano .NET Framework 4,5 sono supportate in Windows Vista o versioni successive.

Oltre ai requisiti del sistema operativo e di .NET Framework, i trasporti disponibili per SignalR presentano requisiti specifici. I trasporti seguenti sono supportati nelle configurazioni seguenti:

**Requisiti per il trasporto desktop e Silverlight di Windows**

| Trasporto | Applicazione .NET | Silverlight |
| --- | --- | --- |
| WebSocket | Windows 8 + e .NET 4.5 + | N/D |
| Frame Forever | N/D | N/D |
| Eventi inviati dal server | .NET 4+ | 5+ |
| Polling prolungato | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Applicazioni Windows Store e Windows Phone

SignalR può essere usato nelle applicazioni di Windows Store e nelle applicazioni Windows Phone 8. I trasporti seguenti sono supportati nelle configurazioni seguenti:

**Requisiti del trasporto di Windows Store e Windows Phone**

| Trasporto | Windows Store/.NET | Windows Store/JavaScript | Windows Phone/IE | Windows Phone/.NET |
| --- | --- | --- | --- | --- |
| Oggetti WebSocket | N/D | Win8 + | 8+ | N/D |
| Frame Forever | N/D | Win8 + | 7.5+ | N/D |
| Eventi inviati dal server | Win8 + | N/D | N/D | 8+ |
| Polling prolungato | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Aggiornamenti consigliati

Per i server SignalR sono consigliati gli aggiornamenti seguenti:

- Un aggiornamento per .NET Framework 4,5 è disponibile [qui](https://support.microsoft.com/kb/2750149).
- Microsoft rilascerà periodicamente QFE per ASP.NET. Questi devono essere applicati come disponibili.
