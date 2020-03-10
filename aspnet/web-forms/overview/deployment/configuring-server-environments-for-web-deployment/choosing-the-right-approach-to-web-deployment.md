---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Scelta dell'approccio corretto per la distribuzione Web | Microsoft Docs
author: jrjlee
description: Quando si utilizza lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web) 2,0 o versione successiva, è possibile utilizzare tre approcci principali per ottenere...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13f784dd8e6404806104d56b026b3c41ca178892
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548482"
---
# <a name="choosing-the-right-approach-to-web-deployment"></a>Scelta dell'approccio corretto per la distribuzione Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Quando si utilizza lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web) 2,0 o versione successiva, è possibile utilizzare tre approcci principali per creare applicazioni Web in pacchetto in un server Web. È possibile:
> 
> - Distribuire l'applicazione da una posizione remota impostando come destinazione il *servizio Web Deployment Agent* (anche noto come "agente remoto") nel server di destinazione.
> - Distribuire l'applicazione da una posizione remota usando Distribuzione Web on demand, nota anche come "agente temporaneo".
> - Distribuire l'applicazione da una posizione remota impostando come destinazione il *gestore di distribuzione Web IIS* nel server di destinazione.
> - Per distribuire l'applicazione, copiare manualmente il pacchetto Web nel server di destinazione e importarlo tramite Gestione IIS.
> 
> Il modo in cui vengono configurati i server Web di destinazione dipende da quale approccio alla distribuzione si vuole usare. Questo argomento consente di scegliere l'approccio più adatto alla distribuzione.

Questa tabella illustra i principali vantaggi e svantaggi di ogni approccio di distribuzione, insieme agli scenari che in genere soddisfano ogni approccio.

| Approccio | Vantaggi | Svantaggi | Scenari tipici |
| --- | --- | --- | --- |
| Agente remoto | È facile da configurare. È adatto per aggiornamenti regolari per le applicazioni Web e il contenuto. | L'utente deve essere un amministratore nel server di destinazione. l'utente non può fornire credenziali alternative. | Ambienti di sviluppo. Ambienti di test. |
| Agente temporaneo | Non è necessario installare Distribuzione Web nel computer di destinazione. Viene automaticamente utilizzata la versione più recente di Distribuzione Web. | L'utente deve essere un amministratore nel server di destinazione. l'utente non può fornire credenziali alternative. | Ambienti di sviluppo. Ambienti di test. |
| Gestore Distribuzione Web | Gli utenti non amministratori possono distribuire il contenuto. È adatto per aggiornamenti regolari per le applicazioni Web e il contenuto. | È molto più complesso da configurare. | Ambienti di staging. Ambienti di produzione Intranet. Ambienti ospitati. |
| Distribuzione offline | È molto facile da configurare. È adatto per ambienti isolati. | L'amministratore del server deve copiare e importare manualmente il pacchetto Web ogni volta. | Ambienti di produzione con connessione Internet. Ambienti di rete isolati. |

## <a name="using-the-remote-agent"></a>Uso dell'agente remoto

Quando si installa Distribuzione Web utilizzando le impostazioni predefinite di un server di destinazione, il servizio Deployment Agent Web ("agente remoto") viene installato e avviato automaticamente. Per impostazione predefinita, l'agente remoto espone un endpoint HTTP a questo indirizzo:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]

> [!NOTE]
> È possibile sostituire [*Server*] con il nome del computer del server Web, un indirizzo IP per il server Web o un nome host che si risolve nel server Web.

Gli amministratori del server possono distribuire i pacchetti Web da una posizione remota, ad esempio un computer di sviluppo o un server di compilazione, specificando questo indirizzo endpoint. Si supponga, ad esempio, che Matt Hink in Fabrikam, Inc. abbia compilato il progetto di applicazione Web ContactManager. Mvc nel computer di sviluppo. Il processo di compilazione genera un pacchetto Web, insieme a un file *. deploy. cmd* che contiene i comandi distribuzione Web necessari per installare il pacchetto. Se Matt è un amministratore del server nel server TESTWEB1, può distribuire l'applicazione Web nel server Web di test eseguendo questo comando nel computer di sviluppo:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]

In effetti, il Distribuzione Web eseguibile può dedurre l'indirizzo dell'endpoint dell'agente remoto se si specifica il nome del computer, quindi Matt deve solo digitare quanto segue:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]

> [!NOTE]
> Per ulteriori informazioni sulla sintassi della riga di comando Distribuzione Web e sui file *. deploy. cmd* , vedere [procedura: installare un pacchetto di distribuzione utilizzando il file Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

L'agente remoto offre un modo semplice per distribuire il contenuto da una posizione remota e questo approccio può funzionare correttamente con una distribuzione automatizzata o con un clic. Tuttavia, l'utente che esegue il comando di distribuzione deve essere anche un amministratore di dominio o un membro del gruppo Administrators locale nel server di destinazione. Inoltre, l'agente remoto non supporta l'autenticazione di base, pertanto non è possibile passare credenziali alternative nella riga di comando.

L'agente remoto offre un approccio utile alla distribuzione negli scenari di sviluppo o di test, in cui non è insolito per gli sviluppatori disporre di un controllo completo dell'amministratore su un ambiente server di prova e le applicazioni vengono in genere ricompilate e ridistribuite frequente. Tuttavia, questo approccio è in genere meno accettabile per gli ambienti di gestione temporanea o di produzione.

Per un esempio end-to-end di uno scenario che usa l'approccio dell'agente remoto, vedere [scenario: configurazione di un ambiente di test per la distribuzione Web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Uso dell'agente temporaneo

L'approccio dell'agente temporaneo alla distribuzione è simile a quello dell'agente remoto. Tuttavia, a differenza dell'approccio dell'agente remoto, non è necessario installare Distribuzione Web nel server Web di destinazione. Al contrario, quando si esegue la distribuzione, Distribuzione Web installerà una versione temporanea del servizio agente di distribuzione Web nel server di destinazione e la utilizzerà per distribuire il contenuto in IIS. Una volta completata la distribuzione, vengono rimossi tutti i file temporanei.

Se si desidera utilizzare l'impostazione del provider agente temporaneo, aggiungere il flag **/g** al comando di distribuzione:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]

> [!NOTE]
> Non è possibile utilizzare l'agente temporaneo se il servizio agente distribuzione Web è installato nel computer di destinazione, anche se il servizio non è in esecuzione.

Il vantaggio di questo approccio è che non è necessario mantenere le installazioni di Distribuzione Web nei server di destinazione. Non è inoltre necessario assicurarsi che i computer di origine e di destinazione eseguano la stessa versione di Distribuzione Web. Questo approccio, tuttavia, presenta le stesse limitazioni principali dell'approccio dell'agente remoto, ovvero è necessario essere un amministratore locale nel server di destinazione per distribuire il contenuto ed è supportata solo l'autenticazione NTLM. L'approccio dell'agente temporaneo richiede anche una configurazione iniziale molto maggiore dell'ambiente di destinazione.

Per altre informazioni sull'uso dell'agente temporaneo, vedere [procedura: installare un pacchetto di distribuzione mediante il file Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx) e [distribuzione Web on demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Uso del gestore Distribuzione Web

Per IIS 7 e versioni successive, Distribuzione Web offre un approccio di distribuzione alternativo tramite il gestore di Distribuzione Web IIS. Il gestore Distribuzione Web è strettamente integrato con il servizio gestione Web IIS (gestione Web), progettato per consentire agli utenti di gestire siti Web IIS da posizioni remote.

Per impostazione predefinita, l'agente remoto espone un endpoint HTTP a questo indirizzo:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]

> [!NOTE]
> È possibile sostituire [*Server*] con il nome del computer del server Web, un indirizzo IP per il server Web o un nome host che si risolve nel server Web.

Il grande vantaggio del gestore Distribuzione Web sull'agente remoto e l'agente temporaneo è che è possibile configurare IIS per consentire agli utenti non amministratori di distribuire le applicazioni e il contenuto a specifici siti Web IIS. Il gestore Distribuzione Web supporta anche l'autenticazione di base, pertanto è possibile specificare credenziali alternative come parametri nei comandi di Distribuzione Web. Il principale svantaggio è che il gestore Distribuzione Web è inizialmente molto più complicato da installare e configurare.

Nel caso di utenti non amministratori, il servizio di gestione Web (gestione Web) consentirà all'utente di connettersi a IIS solo tramite una connessione a livello di sito, anziché una connessione a livello di server. Per accedere a un sito specifico, è possibile includere una stringa di query specifica del sito nell'indirizzo endpoint:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]

Si supponga, ad esempio, che un processo di compilazione sia configurato per distribuire automaticamente un'applicazione Web in un ambiente di gestione temporanea dopo ogni compilazione completata. Se è stato usato l'approccio dell'agente remoto, è necessario rendere l'identità del processo di compilazione un amministratore nei server di destinazione. Al contrario, utilizzando l'approccio del gestore distribuzione Web è possibile concedere a un utente&#x2014;non amministratore**FABRIKAM\stagingdeployer** in questo&#x2014;caso l'autorizzazione solo a un sito Web IIS specifico e il processo di compilazione può fornire tali credenziali per distribuire il pacchetto Web.

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]

> [!NOTE]
> Per ulteriori informazioni sulla sintassi e sulle operazioni della riga di comando Distribuzione Web, vedere [distribuzione Web riferimento alla riga di comando](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Per altre informazioni sull'uso del file *. deploy. cmd* , vedere [procedura: installare un pacchetto di distribuzione usando il file Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Il gestore Distribuzione Web offre un approccio utile alla distribuzione negli ambienti di gestione temporanea, negli ambienti ospitati e negli ambienti di produzione basati su Intranet, in cui l'accesso remoto al server è disponibile, ma non le credenziali di amministratore.

Per un esempio end-to-end di uno scenario che usa l'approccio del gestore Distribuzione Web, vedere [scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Uso della distribuzione offline

In alcuni casi non è possibile o pratico distribuire le applicazioni e il contenuto in un sito Web IIS da una posizione remota. Ad esempio, i computer di origine e di destinazione possono trovarsi in reti isolate o in segmenti di rete oppure i criteri firewall potrebbero non consentire l'accesso remoto.

In scenari come questi, è comunque possibile usare le funzionalità di creazione di pacchetti e pubblicazione di Distribuzione Web; non è possibile usarli da una posizione remota. Un amministratore del server di destinazione deve invece copiare il pacchetto Web nel server e importarlo tramite Gestione IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

L'approccio di distribuzione offline è in genere utile negli ambienti di produzione con connessione Internet, in cui i server in una rete perimetrale possono avere una connettività limitata con i computer nella rete interna.

Per un esempio end-to-end di uno scenario che usa l'approccio di distribuzione offline, vedere [scenario: configurazione di un ambiente di produzione per la distribuzione Web](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla sintassi e sulle operazioni della riga di comando Distribuzione Web, vedere [distribuzione Web riferimento alla riga di comando](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Per altre informazioni sull'uso del file *. deploy. cmd* , vedere [procedura: installare un pacchetto di distribuzione usando il file Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Per indicazioni più generali sui diversi modi in cui è possibile distribuire i pacchetti Web da un computer remoto, vedere l'articolo relativo all' [uso di distribuzione Web in modalità remota](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Per ulteriori informazioni sull'utilizzo di Distribuzione Web su richiesta, vedere [distribuzione Web su richiesta](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Precedente](configuring-server-environments-for-web-deployment.md)
> [Successivo](scenario-configuring-a-test-environment-for-web-deployment.md)
