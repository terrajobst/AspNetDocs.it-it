---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Distribuzione Web aziendale: Panoramica dello scenario | Microsoft Docs'
author: jrjlee
description: Questo set di esercitazioni usa una soluzione di esempio con un livello di complessità realistico, insieme a uno scenario di distribuzione aziendale fittizio, per fornire un riferimento...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 9786879844da13c21e6a953b1ab24b29ca8121e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574130"
---
# <a name="enterprise-web-deployment-scenario-overview"></a>Distribuzione Web aziendale: Panoramica dello scenario

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo set di esercitazioni usa una soluzione di esempio con un livello di complessità realistico, insieme a uno scenario di distribuzione aziendale fittizio, per fornire un'implementazione di riferimento e per fornire le attività e le procedure dettagliate di un contesto comune. In questo argomento viene descritto lo scenario dell'esercitazione e viene introdotta la soluzione di esempio.

## <a name="scenario-description"></a>Descrizione dello scenario

Fabrikam, Inc., una società fittizia, sta creando una soluzione che consente ai team di vendita remoti di archiviare e recuperare le informazioni di contatto da un'interfaccia Web.

I processi di Application Lifecycle Management (ALM) in Fabrikam, Inc. richiedono che la soluzione venga distribuita in tre ambienti server in diverse fasi del processo di sviluppo del software:

- Un ambiente di testing per sviluppatori o "sandbox".
- Ambiente di gestione temporanea basato su Intranet.
- Ambiente di produzione con connessione Internet.

Ognuno di questi ambienti presenta requisiti di configurazione e di sicurezza diversi e ognuno pone problemi di distribuzione univoci.

### <a name="the-fabrikam-inc-server-infrastructure"></a>Infrastruttura di Fabrikam, Inc. Server

Si tratta dell'infrastruttura di sviluppo e distribuzione di alto livello di Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Le workstation per sviluppatori, l'infrastruttura di controllo del codice sorgente, l'ambiente di test per sviluppatori e l'ambiente di gestione temporanea si trovano tutti nella rete Intranet all'interno del dominio Fabrikam.net. L'ambiente di produzione risiede in una rete perimetrale (nota anche come subnet schermata), che è isolata dalla rete Intranet da un firewall. Si tratta di uno scenario di distribuzione comune: i server Web con connessione Internet vengono in genere isolati dall'infrastruttura server interna tramite l'utilizzo di firewall o server gateway.

In questo esempio:

- Un server Team Foundation Server (TFS) 2010 con un server di compilazione separato fornisce funzionalità di controllo del codice sorgente e integrazione continua (CI).
- L'ambiente di test per sviluppatori include un server Web Internet Information Services (IIS) 7,5 e un server di database SQL Server 2008 R2.
- L'ambiente di produzione include più server Web IIS 7,5 sincronizzati da un server del controller WFF (Web Farm Framework), insieme a un server di database SQL Server 2008 R2. In pratica, il server di database può utilizzare il clustering o il mirroring per migliorare la scalabilità e la disponibilità.
- L'ambiente di gestione temporanea è progettato per replicare il più possibile la configurazione dell'ambiente di produzione.
- Il firewall e i criteri di isolamento rete non consentono la distribuzione automatica diretta dalla Intranet alla rete perimetrale.

La configurazione di ognuno di questi ambienti è descritta più dettagliatamente nella seconda esercitazione Configurazione degli [ambienti server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Ruoli del team per ALM

Questi utenti sono interessati alla creazione, alla gestione, alla compilazione e alla pubblicazione della soluzione Contact Manager:

- Matt Hink è uno sviluppatore di applicazioni Web di Fabrikam, Inc. Fa parte del team che ha sviluppato la soluzione Contact Manager con Visual Studio 2010. Matt dispone di diritti di amministratore completi per i server nell'ambiente di test per sviluppatori, che consente di configurare l'ambiente in base alle proprie esigenze. Dispone inoltre dell'accesso utente all'istanza di Visual Studio 2010 TFS in cui archivia il codice sorgente per la soluzione Contact Manager.
- Rob Walters è un amministratore del server per il team di sviluppo Fabrikam, Inc. Rob dispone di accesso amministrativo al server TFS in modo da poter configurare tutti gli aspetti di TFS e Team Build. Rob dispone inoltre dell'accesso amministrativo ai server Web di test e di gestione temporanea e funge da amministratore di database (DBA) per i server di database negli ambienti di test e di gestione temporanea. Rob ha configurato Team Build nel server TFS per eseguire queste attività:

    - Compilare ed eseguire unit test sull'applicazione ogni volta che un utente archivia un file in TFS. Questa operazione è denominata CI.
    - Distribuire automaticamente l'applicazione Contact Manager nell'ambiente di test dopo che l'applicazione ha superato gli unit test. Ciò include la pubblicazione del database nei server di prova durante la distribuzione iniziale ed eventuali aggiornamenti al database dopo la distribuzione iniziale.
    - Distribuire l'applicazione Contact Manager nell'ambiente di gestione temporanea in un processo a un solo passaggio.
    - Creare un pacchetto Web che può essere utilizzato da un amministratore del server Web e da un amministratore di database per pubblicare l'applicazione nell'ambiente di produzione.
- Lisa Andrews è un amministratore del server responsabile della distribuzione di applicazioni nei server Fabrikam, Inc. Production. Ha accesso in lettura alla condivisione in cui il Team Build di TFS archivia il pacchetto di distribuzione Web dopo aver compilato l'applicazione Contact Manager. Dispone inoltre dell'accesso amministrativo ai server Web di produzione, in modo da poter distribuire l'applicazione in produzione. Inoltre, agisce da DBA che distribuisce i database e gli aggiornamenti del database nel server di database nell'ambiente di produzione.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Soluzione Contact Manager

La soluzione Contact Manager è progettata per consentire agli utenti registrati e registrati di aggiungere e modificare le informazioni di contatto tramite un'interfaccia Web. La soluzione Contact Manager è costituita da quattro progetti singoli:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager. Mvc**. Si tratta di un progetto di applicazione Web MVC3 ASP.NET che rappresenta il punto di ingresso per la soluzione. Offre funzionalità di base per le applicazioni Web, ad esempio per fornire agli utenti la possibilità di creare e visualizzare i dettagli dei contatti. L'applicazione si basa su un servizio Windows Communication Foundation (WCF) per gestire i contatti e un database dei servizi dell'applicazione ASP.NET per gestire l'autenticazione e l'autorizzazione.
- **ContactManager. database**. Si tratta di un progetto di database di Visual Studio 2010. Il progetto definisce lo schema per un database in cui sono archiviati i dettagli del contatto.
- **ContactManager. Service**. Si tratta di un progetto di servizio Web WCF. WCF espone un endpoint che consente ai chiamanti di eseguire operazioni di creazione, recupero, aggiornamento ed eliminazione (CRUD) nel database Contact Manager. Il servizio si basa sul database Contact Manager e sull'assembly ContactManager. Common. dll.
- **ContactManager. Common**. Si tratta di un progetto libreria di classi. Il servizio WCF si basa sui tipi definiti in questo assembly.

Nella prima esercitazione di questa serie, [distribuzione Web nell'azienda](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md), viene fornita una panoramica completa della soluzione e dei relativi requisiti di distribuzione.

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Attività di distribuzione

Sono disponibili diverse attività distinte per la distribuzione di applicazioni in ambienti diversi in un'organizzazione di grandi dimensioni. Queste sono le attività principali trattate dalle esercitazioni:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Di seguito è riportato un elenco di tutti i passaggi del processo di distribuzione dal punto di vista degli utenti descritti in precedenza in questo documento:

1. Tutti i membri del team verificano la soluzione Contact Manager in Visual Studio 2010 per determinare i requisiti e i problemi di distribuzione chiave.
2. Matt Hink può distribuire la soluzione Contact Manager direttamente dalla workstation per sviluppatori all'ambiente di test per sviluppatori, per condurre un test iniziale della logica di distribuzione.
3. Matt Hink aggiunge l'applicazione al controllo del codice sorgente in TFS.
4. Rob Walters crea varie definizioni di compilazione per la soluzione Contact Manager in Team Build. Una definizione di compilazione USA CI per distribuire la soluzione nell'ambiente di test per sviluppatori ogni volta che un utente archivia un nuovo codice. Un'altra definizione di compilazione consente agli utenti di attivare le distribuzioni nell'ambiente di gestione temporanea secondo necessità.
5. Ogni volta che un utente archivia un nuovo codice, Team Build compila automaticamente i componenti della soluzione, esegue unit test e distribuisce la soluzione nell'ambiente di test per sviluppatori se la compilazione ha avuto esito positivo e gli unit test sono stati superati.
6. Quando un utente attiva una distribuzione nell'ambiente di gestione temporanea, la soluzione viene assemblata e distribuita in un processo a un solo passaggio. Questo processo genera anche un pacchetto per la distribuzione manuale nell'ambiente di produzione.
7. Lisa Andrews distribuisce l'applicazione nell'ambiente di produzione importando manualmente il pacchetto Web creato nel passaggio 6.

### <a name="key-deployment-issues"></a>Problemi di distribuzione principali

La soluzione Contact Manager e lo scenario Fabrikam, Inc. evidenziano vari problemi comuni e sfide che possono verificarsi quando si distribuiscono soluzioni complesse di livello aziendale. Esempio:

- È necessario essere in grado di distribuire progetti in più ambienti, ad esempio gli ambienti di sviluppo o test, le piattaforme di staging e i server di produzione. La soluzione deve essere distribuita con impostazioni di configurazione diverse per ogni ambiente.
- È necessario distribuire simultaneamente più progetti dipendenti come parte di un processo di compilazione e distribuzione automatizzato in un singolo passaggio.
- È necessario essere in grado di gestire la distribuzione da un processo automatizzato. Ad esempio, si vuole usare un processo CI per distribuire le applicazioni Web in un ambiente di gestione temporanea quando viene archiviato nuovo codice.
- È necessario essere in grado di controllare il processo di distribuzione e impostare le variabili di distribuzione dall'esterno di Visual Studio, poiché è improbabile che gli sviluppatori abbiano le impostazioni di configurazione corrette o le credenziali necessarie per ogni ambiente di destinazione.
- È necessario distribuire progetti di database basati su schema e mantenere i dati esistenti nelle distribuzioni successive.
- È necessario distribuire i database di appartenenza su base ad hoc senza distribuire i dati dell'account utente. Potrebbe inoltre essere necessario aggiornare lo schema dei database di appartenenza distribuiti senza perdere i dati dell'account utente esistente.
- È necessario escludere determinati file o cartelle quando si distribuisce il contenuto in diversi ambienti di destinazione.

Inoltre, la gestione della distribuzione quando gli aggiornamenti sono frequenti e incrementale genera ulteriori problemi. Esempio:

- Gli unit test vengono eseguiti ogni volta che uno sviluppatore archivia un nuovo codice. Si desidera distribuire la soluzione solo se il codice supera gli unit test.
- Quando si distribuisce un'applicazione Web in un ambiente di gestione temporanea o di produzione, si desidera reindirizzare gli utenti a un' *app\_file offline. htm* per la durata del processo di distribuzione.
- Si desidera registrare le attività di distribuzione. Il processo di distribuzione deve inviare notifiche tramite posta elettronica di distribuzioni con esito positivo o negativo ai destinatari designati.
- Se una distribuzione automatica ha esito negativo, il processo di distribuzione deve ritentare la distribuzione corrente o distribuire invece il precedente pacchetto Web.

> [!div class="step-by-step"]
> [Precedente](deploying-web-applications-in-enterprise-scenarios.md)
> [Successivo](application-lifecycle-management-from-development-to-production.md)
