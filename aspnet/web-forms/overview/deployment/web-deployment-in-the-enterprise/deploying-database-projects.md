---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Distribuzione di progetti di database | Microsoft Docs
author: jrjlee
description: "Nota: in molti scenari di distribuzione aziendali è necessario avere la possibilità di pubblicare aggiornamenti incrementali in un database distribuito. L'alternativa consiste nel ricreare..."
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 221808758492aedb8e8329364e511df28fd11105
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634267"
---
# <a name="deploying-database-projects"></a>Distribuzione di progetti di database

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> In molti scenari di distribuzione aziendali è necessario avere la possibilità di pubblicare aggiornamenti incrementali in un database distribuito. L'alternativa consiste nel ricreare il database in ogni distribuzione, il che significa che si perdono tutti i dati nel database esistente. Quando si lavora con Visual Studio 2010, l'uso di VSDBCMD è l'approccio consigliato alla pubblicazione incrementale del database. Tuttavia, la versione successiva di Visual Studio e la pipeline di pubblicazione Web (WPP) includeranno gli strumenti che supportano direttamente la pubblicazione incrementale.

Se si apre la soluzione di esempio Contact Manager in Visual Studio 2010, si noterà che il progetto di database include una cartella Properties che contiene quattro file.

![](deploying-database-projects/_static/image1.png)

Insieme al file di progetto (*ContactManager. database. dbproj* in questo caso), questi file controllano vari aspetti del processo di compilazione e distribuzione:

- Il file *database. sqlcmdvars* fornisce i valori per tutte le variabili SQLCMD usate quando si distribuisce il progetto. Ogni configurazione di soluzione (ad esempio, debug e Release) può specificare un file con estensione sqlcmdvars diverso.
- Il file *database. sqldeployment* fornisce impostazioni specifiche della distribuzione, ad esempio se utilizzare le regole di confronto definite nel progetto o le regole di confronto del server di destinazione, se ricreare il database di destinazione ogni volta o semplicemente modificare il database esistente per renderlo aggiornato e così via. Ogni configurazione di soluzione può specificare un file. sqldeployment diverso.
- Il file *database. sqlpermissions* è un documento XML che è possibile utilizzare per definire le autorizzazioni che si desidera aggiungere al database di destinazione. Tutte le configurazioni della soluzione condividono lo stesso file. sqlpermissions.
- Il file *database. sqlsettings* specifica le proprietà a livello di database da utilizzare durante la creazione del database, ad esempio le regole di confronto da utilizzare, il comportamento degli operatori di confronto e così via. Tutte le configurazioni della soluzione condividono lo stesso file sqlsettings.

È opportuno aprire questi file in Visual Studio e acquisire familiarità con il contenuto.

Quando si compila un progetto di database, il processo di compilazione crea due file:

- Uno *schema di database* (file con estensione dbschema). Viene descritto lo schema del database che si desidera creare in formato XML.
- Un *manifesto di distribuzione* (file con estensione deploymanifest). Contiene tutte le informazioni necessarie per creare e distribuire il database. Fa riferimento al file con estensione dbschema insieme ad altre risorse, ad esempio le istruzioni di distribuzione (il file. sqldeployment) e gli script SQL di pre-distribuzione o post-distribuzione.

Viene visualizzata la relazione tra queste risorse:

![](deploying-database-projects/_static/image2.png)

Come si può notare, il file. sqlsettings e il file. sqlpermissions sono input per il processo di compilazione. Insieme al file di progetto di database, questi file vengono utilizzati per creare il file di schema del database. Il file. sqldeployment e il file. sqlcmdvars passano il processo di compilazione senza modifiche. Il manifesto della distribuzione indica il percorso dello schema del database, il file. sqldeployment, il file con estensione sqlcmdvars e gli script SQL di pre-distribuzione o post-distribuzione.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Perché usare VSDBCMD per distribuire un progetto di database?

Esistono diversi approcci alla distribuzione di progetti di database. Tuttavia, non tutte sono adatte per la distribuzione di un progetto di database a server remoti in un ambiente aziendale. Considerare gli elementi desiderati dalla distribuzione di un progetto di database. Negli scenari di distribuzione aziendali è probabile che si desideri:

- Possibilità di distribuire il progetto di database da una posizione remota.
- Possibilità di eseguire aggiornamenti incrementali a un database esistente.
- Possibilità di includere gli script di pre-distribuzione o di post-distribuzione.
- La possibilità di personalizzare la distribuzione in più ambienti di destinazione.
- La possibilità di distribuire il progetto di database come parte di una distribuzione della soluzione in un unico passaggio di dimensioni più grandi, in genere basata su script.

Esistono tre approcci principali che è possibile utilizzare per distribuire un progetto di database:

- È possibile usare la funzionalità di distribuzione con il tipo di progetto di database in Visual Studio 2010. Quando si compila e si distribuisce un progetto di database in Visual Studio 2010, il processo di distribuzione usa il manifesto di distribuzione per generare un file di distribuzione basato su SQL specifico per la configurazione della build. In questo modo verrà creato il database se non esiste già o apportare le modifiche necessarie al database, se esistente. È possibile utilizzare SQLCMD. exe per eseguire il file nel server di destinazione oppure è possibile impostare Visual Studio per creare ed eseguire il file. Lo svantaggio di questo approccio è la possibilità di avere un controllo limitato sulle impostazioni di distribuzione. Spesso è anche necessario modificare il file di distribuzione SQL per fornire valori di variabili specifici dell'ambiente. È possibile usare questo approccio solo da un computer in cui è installato Visual Studio 2010 e lo sviluppatore deve essere in grado di fornire le stringhe di connessione e le credenziali per tutti gli ambienti di destinazione.
- È possibile utilizzare lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web) per [distribuire un database come parte di un progetto di applicazione Web](https://msdn.microsoft.com/library/dd465343.aspx). Tuttavia, questo approccio è molto più complesso se si desidera distribuire un progetto di database anziché replicare semplicemente un database locale esistente in un server di destinazione. È possibile configurare Distribuzione Web per eseguire lo script di distribuzione SQL generato dal progetto di database, ma a tale scopo è necessario creare un file di destinazioni WPP personalizzato per il progetto di applicazione Web. In questo modo viene aggiunta una notevole quantità di complessità al processo di distribuzione. Inoltre, Distribuzione Web non supporta direttamente gli aggiornamenti incrementali per i database esistenti. Per ulteriori informazioni su questo approccio, vedere [estensione della pipeline di pubblicazione Web al progetto di database del pacchetto file SQL distribuito](https://go.microsoft.com/?linkid=9805121).
- È possibile usare l'utilità VSDBCMD per distribuire il database, usando lo schema del database o il manifesto della distribuzione. È possibile chiamare VSDBCMD. exe da una destinazione MSBuild, che consente di pubblicare i database come parte di un processo di distribuzione con script di dimensioni maggiori. È possibile eseguire l'override delle variabili nel file con estensione sqlcmdvars e di molte altre proprietà del database da un comando VSDBCMD, che consente di personalizzare la distribuzione per ambienti diversi senza creare più configurazioni di compilazione. VSDBCMD fornisce funzionalità di differenziazione, ovvero apporterà solo le modifiche necessarie per allineare un database di destinazione allo schema del database. VSDBCMD offre inoltre un'ampia gamma di opzioni della riga di comando, che offrono un controllo accurato sul processo di distribuzione.

Da questa panoramica, è possibile notare che l'uso di VSDBCMD con MSBuild è l'approccio più adatto a uno scenario di distribuzione aziendale tipico:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Supporta la distribuzione remota? | Yes | Yes | Yes |
| Supporta gli aggiornamenti incrementali? | Yes | No | Yes |
| Supporta gli script pre/post-distribuzione? | Yes | Yes | Yes |
| Supporta la distribuzione in più ambienti? | Limitato | Limitato | Yes |
| Supporta la distribuzione con script? | Limitato | Yes | Yes |

Nella parte restante di questo argomento viene descritto l'utilizzo di VSDBCMD con MSBuild per distribuire progetti di database.

## <a name="understanding-the-deployment-process"></a>Informazioni sul processo di distribuzione

L'utilità VSDBCMD consente di distribuire un database usando lo schema del database (file con estensione dbschema) o il manifesto di distribuzione (il file con estensione deploymanifest). In pratica, si userà quasi sempre il manifesto della distribuzione, perché il manifesto della distribuzione consente di fornire i valori predefiniti per le varie proprietà di distribuzione e di identificare gli script SQL di pre-distribuzione o post-distribuzione che si desidera eseguire. Ad esempio, questo comando VSDBCMD viene usato per distribuire il database **ContactManager** in un server di database in un ambiente di testing:

[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]

In questo caso:

- L'opzione **/a** (o **/Action**) specifica gli elementi che si desidera eseguire VSDBCMD. È possibile impostare questa impostazione per l' **importazione** o la **distribuzione**. L'opzione di **importazione** viene utilizzata per generare un file con estensione dbschema da un database esistente e l'opzione **Distribuisci** viene utilizzata per distribuire un file con estensione dbschema in un database di destinazione.
- L'opzione **/manifest** (o **/ManifestFile**) identifica il file con estensione deploymanifest che si desidera distribuire. Se invece si desidera utilizzare il file con estensione dbschema, utilizzare l'opzione **/Model** (o **/ModelFile**).
- L'opzione **/CS** (o **/ConnectionString**) fornisce la stringa di connessione per il server di database di destinazione. Si noti che questo non include il nome del database&#x2014;VSDBCMD deve connettersi al server per creare il database. non è necessario connettersi a un singolo database. Se il file con estensione deploymanifest include una stringa di connessione, è possibile omettere questa opzione. Se si usa l'opzione comunque, il valore dell'opzione eseguirà l'override del valore. DeployManifest.
- La proprietà <strong>/p: TargetDatabase</strong> fornisce il nome che si desidera assegnare al database di destinazione durante la creazione. Viene eseguito l'override del valore della proprietà <strong>TargetDatabase</strong> nel file. DeployManifest. È possibile utilizzare la sintassi <strong>/p:</strong> <em>[Property Name]</em>per impostare un'ampia varietà di proprietà di distribuzione ed eseguire l'override di qualsiasi variabile SQLCMD dichiarata nel file con estensione sqlcmdvars.
- L'opzione **/DD +** (o **/DeployToDatabase +** ) indica che si desidera creare una distribuzione e distribuirla nell'ambiente di destinazione. Se si specifica **/DD-** o si omette l'opzione, VSDBCMD genererà uno script di distribuzione, ma non lo distribuirà nell'ambiente di destinazione. Questa opzione è spesso la fonte di confusione ed è illustrata più dettagliatamente nella sezione successiva.
- L'opzione **/script** (o **/DeploymentScriptFile**) specifica la posizione in cui si desidera generare lo script di distribuzione. Questo valore non influisce sul processo di distribuzione.

Per ulteriori informazioni su VSDBCMD, vedere [la Guida di riferimento alla riga di comando per VSDBCMD. EXE (distribuzione e importazione dello schema)](https://msdn.microsoft.com/library/dd193283.aspx) e [procedura: preparare un database per la distribuzione da un prompt dei comandi usando VSDBCMD. File EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Per un esempio di come è possibile usare VSDBCMD da un file di progetto MSBuild, vedere [informazioni sul processo di compilazione](understanding-the-build-process.md). Per esempi di come configurare le impostazioni di distribuzione del database per più ambienti, vedere [personalizzazione delle distribuzioni di database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Informazioni sull'opzione DeployToDatabase

Il comportamento dell'opzione **/DD** o **/DeployToDatabase** varia a seconda che si stia usando VSDBCMD con un file con estensione dbschema o con estensione DeployManifest. Se si usa un file con estensione dbschema, il comportamento è piuttosto semplice:

- Se si specifica **/DD +** o **/DD**, VSDBCMD genererà uno script di distribuzione e distribuirà il database.
- Se si specifica **/DD-** o si omette l'opzione, VSDBCMD genererà solo uno script di distribuzione.

Se si usa un file con estensione deploymanifest, il comportamento è molto più complesso. Questo perché il file. DeployManifest contiene un nome di proprietà **DeployToDatabase** che determina anche se il database è distribuito.

[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]

Il valore di questa proprietà viene impostato in base alle proprietà del progetto di database. Se si imposta l' **azione Distribuisci** per **creare uno script di distribuzione (con estensione SQL)** , il valore sarà **false**. Se si imposta l' **azione Distribuisci** per **creare uno script di distribuzione (con estensione SQL) e si esegue la distribuzione nel database**, il valore sarà **true**.

> [!NOTE]
> Queste impostazioni sono associate a una configurazione e una piattaforma di compilazione specifiche. Se, ad esempio, si configurano le impostazioni per la configurazione di **debug** e quindi si esegue la pubblicazione con la configurazione di **rilascio** , le impostazioni non verranno usate.

![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> In questo scenario, l' **azione Distribuisci** deve essere sempre impostata per **creare uno script di distribuzione (con estensione SQL)** , perché non si vuole che Visual Studio 2010 distribuisca il database. In altre parole, la proprietà **DeployToDatabase** deve essere sempre **false**.

Quando viene specificata una proprietà **DeployToDatabase** , l'opzione **/DD** eseguirà l'override della proprietà solo se il valore della proprietà è **false**:

- Se la proprietà **DeployToDatabase** è **false**e si specifica **/DD +** o **/DD**, VSDBCMD eseguirà l'override della proprietà **DeployToDatabase** e distribuirà il database.
- Se la proprietà **DeployToDatabase** è **false**e si specifica **/DD-** o si omette l'opzione, VSDBCMD non distribuirà il database.
- Se la proprietà **DeployToDatabase** è **true**, VSDBCMD ignorerà l'opzione e distribuirà il database.
- Viene generato uno script di distribuzione in ogni caso, indipendentemente dal fatto che si stia distribuendo anche il database.

## <a name="conclusion"></a>Conclusione

Questo argomento fornisce una panoramica del processo di compilazione e distribuzione per i progetti di database in Visual Studio 2010. Viene inoltre descritto come utilizzare VSDBCMD. exe con MSBuild per supportare la distribuzione di database su scala aziendale.

Per ulteriori informazioni sul funzionamento della procedura, vedere [personalizzazione delle distribuzioni di database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Ulteriori informazioni

Per informazioni su come personalizzare le distribuzioni di database creando un file di configurazione della distribuzione separato per ogni ambiente, vedere [personalizzazione delle distribuzioni di database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Per istruzioni su come configurare le appartenenze ai ruoli del database eseguendo uno script di post-distribuzione, vedere [distribuzione delle appartenenze ai ruoli del database negli ambienti di test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Per istruzioni sulla gestione di alcune delle specifiche esigenze imposte dai database di appartenenza, vedere [distribuzione di database di appartenenza in ambienti aziendali](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

In questi argomenti di MSDN vengono fornite informazioni aggiuntive e informazioni di carattere generale sui progetti di database di Visual Studio e sul processo di distribuzione del database:

- [Progetti di database SQL Server di Visual Studio 2010](https://msdn.microsoft.com/library/ff678491.aspx)
- [Gestione delle modifiche del database](https://msdn.microsoft.com/library/aa833404.aspx)
- [Procedura: preparare un database per la distribuzione da un prompt dei comandi utilizzando VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Panoramica del Compilazione e distribuzione di database](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Precedente](deploying-web-packages.md)
> [Successivo](creating-and-running-a-deployment-command-file.md)
