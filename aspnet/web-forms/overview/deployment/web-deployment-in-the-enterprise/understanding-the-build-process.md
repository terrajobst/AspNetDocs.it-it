---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Informazioni sul processo di compilazione | Microsoft Docs
author: jrjlee
description: Questo argomento fornisce una procedura dettagliata relativa a un processo di compilazione e distribuzione su scala aziendale. L'approccio descritto in questo argomento USA Custom Microsoft Build...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 802d93f7ca987d018967275bae68b8c56d883a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641141"
---
# <a name="understanding-the-build-process"></a>Informazioni sul processo di compilazione

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento fornisce una procedura dettagliata relativa a un processo di compilazione e distribuzione su scala aziendale. L'approccio descritto in questo argomento usa i file di progetto Microsoft Build Engine personalizzati (MSBuild) per fornire un controllo granulare su ogni aspetto del processo. All'interno dei file di progetto, le destinazioni MSBuild personalizzate vengono utilizzate per eseguire utilità di distribuzione come lo strumento di distribuzione Web Internet Information Services (IIS) (MSDeploy. exe) e l'utilità di distribuzione di database VSDBCMD. exe.
> 
> > [!NOTE]
> > Nell'argomento precedente, [informazioni sul file di progetto](understanding-the-project-file.md), sono stati descritti i componenti chiave di un file di progetto MSBuild e sono stati introdotti i concetti relativi ai file di progetto divisi per supportare la distribuzione in più ambienti di destinazione. Se non si ha già familiarità con questi concetti, è consigliabile esaminare [il file di progetto](understanding-the-project-file.md) prima di procedere con questo argomento.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul file di progetto](understanding-the-project-file.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="build-and-deployment-overview"></a>Panoramica di Compilazione e distribuzione

Nella [soluzione Contact Manager](the-contact-manager-solution.md)tre file controllano il processo di compilazione e distribuzione:

- Un *file di progetto universale* (*Publish. proj*). Sono incluse le istruzioni per la compilazione e la distribuzione che non cambiano tra gli ambienti di destinazione.
- Un *file di progetto specifico dell'ambiente* (*env-dev. proj*). Sono incluse le impostazioni di compilazione e distribuzione specifiche di un determinato ambiente di destinazione. Ad esempio, è possibile usare il file *env-dev. proj* per fornire le impostazioni per un ambiente di sviluppo o di test e creare un file alternativo denominato *ENV-stage. proj* per fornire le impostazioni per un ambiente di gestione temporanea.
- Un *file di comando* (*Publish-dev. cmd*). Contiene un comando MSBuild. exe che specifica i file di progetto che si desidera eseguire. È possibile creare un file di comando per ogni ambiente di destinazione, dove ogni file contiene un comando MSBuild. exe che specifica un file di progetto specifico dell'ambiente diverso. Ciò consente allo sviluppatore di eseguire la distribuzione in ambienti diversi semplicemente eseguendo il file di comando appropriato.

Nella soluzione di esempio, è possibile trovare questi tre file nella cartella della soluzione di pubblicazione.

![](understanding-the-build-process/_static/image1.png)

Prima di esaminare questi file in modo più dettagliato, è opportuno esaminare il funzionamento del processo di compilazione globale quando si usa questo approccio. A un livello elevato, il processo di compilazione e distribuzione ha un aspetto simile al seguente:

![](understanding-the-build-process/_static/image2.png)

La prima cosa da fare è che i due file&#x2014;di progetto che contengono le istruzioni di compilazione e distribuzione universale e uno contenente le impostazioni&#x2014;specifiche dell'ambiente vengono uniti in un unico file di progetto. MSBuild usa quindi le istruzioni nel file di progetto. Compila ognuno dei progetti nella soluzione, usando il file di progetto per ogni progetto. Chiama quindi altri strumenti, ad esempio Distribuzione Web (MSDeploy. exe) e l'utilità VSDBCMD per distribuire il contenuto Web e i database nell'ambiente di destinazione.

Dall'inizio alla fine, il processo di compilazione e distribuzione esegue queste attività:

1. Elimina il contenuto della directory di output, in preparazione per una nuova compilazione.
2. Compila ogni progetto nella soluzione:

    1. Per i progetti&#x2014;Web in questo caso, un'applicazione web MVC ASP.NET e un servizio&#x2014;Web WCF il processo di compilazione crea un pacchetto di distribuzione Web per ogni progetto.
    2. Per i progetti di database, il processo di compilazione crea un manifesto di distribuzione (file con estensione deploymanifest) per ogni progetto.
3. Usa l'utilità VSDBCMD. exe per distribuire ogni progetto di database nella soluzione, usando varie proprietà dei file&#x2014;di progetto una stringa di connessione di destinazione e un nome&#x2014;di database insieme al file DeployManifest.
4. Usa l'utilità MSDeploy. exe per distribuire ogni progetto Web nella soluzione, usando varie proprietà dei file di progetto per controllare il processo di distribuzione.

È possibile utilizzare la soluzione di esempio per tracciare questo processo in modo più dettagliato.

> [!NOTE]
> Per istruzioni su come personalizzare i file di progetto specifici dell'ambiente per gli ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="invoking-the-build-and-deployment-process"></a>Richiamo del processo di Compilazione e distribuzione

Per distribuire la soluzione Contact Manager in un ambiente di test per sviluppatori, lo sviluppatore esegue il file di comando *Publish-dev. cmd* . Richiama MSBuild. exe, specificando *Publish. proj* come file di progetto da eseguire e *env-dev. proj* come valore di parametro.

[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]

> [!NOTE]
> L'opzione **/FL** (short for **/FileLogger**) registra l'output di compilazione in un file denominato *MSBuild. log* nella directory corrente. Per ulteriori informazioni, vedere la Guida di [riferimento alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

A questo punto, MSBuild avvia l'esecuzione, carica il file *Publish. proj* e inizia a elaborare le istruzioni al suo interno. La prima istruzione indica a MSBuild di importare il file di progetto specificato dal parametro **TargetEnvPropsFile** .

[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]

Il parametro **TargetEnvPropsFile** specifica il file *env-dev. proj* , quindi MSBuild unisce il contenuto del file *env-dev. proj* al file *Publish. proj* .

Gli elementi successivi che MSBuild rileva nel file di progetto Unito sono gruppi di proprietà. Le proprietà vengono elaborate nell'ordine in cui sono visualizzate nel file. MSBuild crea una coppia chiave-valore per ogni proprietà, a condizione che vengano soddisfatte tutte le condizioni specificate. Le proprietà definite in un secondo momento nel file sovrascriveranno qualsiasi proprietà con lo stesso nome definito in precedenza nel file. Si considerino, ad esempio, le proprietà **OutputRoot** .

[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]

Quando MSBuild elabora il primo elemento **OutputRoot** , specificando un parametro denominato analogo, non è stato specificato, imposta il valore della proprietà **OutputRoot** su **. \Publish\Out**. Quando rileva il secondo elemento **OutputRoot** , se la condizione restituisce **true**, sovrascriverà il valore della proprietà **OutputRoot** con il valore del parametro **OutDir** .

L'elemento successivo che MSBuild rileva è un singolo gruppo di elementi, che contiene un elemento denominato **ProjectsToBuild**.

[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]

MSBuild elabora questa istruzione compilando un elenco di elementi denominato **ProjectsToBuild**. In questo caso, l'elenco di elementi contiene un solo&#x2014;valore il percorso e il nome del file di soluzione.

A questo punto, gli elementi rimanenti sono destinazioni. Le destinazioni vengono elaborate in modo diverso&#x2014;rispetto alle proprietà e agli elementi essenzialmente, le destinazioni non vengono elaborate a meno che non siano specificate esplicitamente dall'utente o richiamate da un altro costrutto all'interno del file di progetto. Si ricordi che il tag del **progetto** di apertura include un attributo **DefaultTargets** .

[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]

Indica a MSBuild di richiamare la destinazione **FullPublish** , se le destinazioni non vengono specificate quando viene richiamato MSBuild. exe. La destinazione **FullPublish** non contiene attività. ma specifica semplicemente un elenco di dipendenze.

[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]

Questa dipendenza indica a MSBuild che, per eseguire la destinazione **FullPublish** , deve richiamare questo elenco di destinazioni nell'ordine indicato:

1. Deve richiamare la destinazione **Clean** .
2. Deve richiamare la destinazione **BuildProjects** .
3. Deve richiamare la destinazione **GatherPackagesForPublishing** .
4. Deve richiamare la destinazione **PublishDbPackages** .
5. Deve richiamare la destinazione **PublishWebPackages** .

### <a name="the-clean-target"></a>Destinazione pulita

La destinazione **pulita** Elimina fondamentalmente la directory di output e tutto il relativo contenuto, come preparazione per una nuova compilazione.

[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]

Si noti che la destinazione include un elemento **ItemGroup** . Quando si definiscono proprietà o elementi all'interno di un elemento di **destinazione** , si creano proprietà ed elementi *dinamici* . In altre parole, le proprietà o gli elementi non vengono elaborati fino a quando non viene eseguita la destinazione. La directory di output potrebbe non esistere o contenere file fino a quando non viene avviato il processo di compilazione, pertanto non è possibile compilare l'elenco **\_FilesToDelete** come elemento statico; è necessario attendere fino a quando l'esecuzione è in corso. Di conseguenza, l'elenco viene compilato come elemento dinamico all'interno della destinazione.

> [!NOTE]
> In questo caso, poiché la destinazione **pulita** è la prima a essere eseguita, non è necessario usare un gruppo di elementi dinamici. Tuttavia, è consigliabile usare le proprietà dinamiche e gli elementi in questo tipo di scenario, perché potrebbe essere necessario eseguire le destinazioni in un ordine diverso in un determinato momento.  
> È inoltre consigliabile evitare di dichiarare elementi che non verranno mai utilizzati. Se si dispone di elementi che verranno utilizzati solo da una destinazione specifica, è consigliabile inserirli nella destinazione per rimuovere eventuali sovraccarichi superflui nel processo di compilazione.

A parte gli elementi dinamici, la destinazione **pulita** è piuttosto semplice e usa le attività incorporate **Message**, **Delete**e **RemoveDir** per:

1. Inviare un messaggio al logger.
2. Compila un elenco di file da eliminare.
3. Eliminare i file.
4. Rimuovere la directory di output.

### <a name="the-buildprojects-target"></a>Destinazione BuildProjects

La destinazione **BuildProjects** compila fondamentalmente tutti i progetti nella soluzione di esempio.

[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]

Questa destinazione è stata descritta in dettaglio nell'argomento precedente, [informazioni sul file di progetto](understanding-the-project-file.md), per illustrare il modo in cui le attività e le destinazioni fanno riferimento a proprietà ed elementi. A questo punto, si è interessati principalmente all'attività **MSBuild** . È possibile utilizzare questa attività per compilare più progetti. L'attività non crea una nuova istanza di MSBuild. exe; Usa l'istanza in esecuzione corrente per compilare ogni progetto. I punti chiave di interesse in questo esempio sono le proprietà di distribuzione:

- La proprietà **DeployOnBuild** indica a MSBuild di eseguire le istruzioni di distribuzione nelle impostazioni del progetto al termine della compilazione di ogni progetto.
- La proprietà **DeployTarget** identifica la destinazione che si desidera richiamare dopo la compilazione del progetto. In questo caso, la destinazione del **pacchetto** compila l'output del progetto in un pacchetto web distribuibile.

> [!NOTE]
> La destinazione del **pacchetto** richiama la pipeline di pubblicazione Web (WPP), che fornisce l'integrazione tra MSBuild e distribuzione Web. Per esaminare le destinazioni predefinite fornite dal WPP, esaminare il file *Microsoft. Web. Publishing. targets* nella cartella% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web.

### <a name="the-gatherpackagesforpublishing-target"></a>Destinazione GatherPackagesForPublishing

Se si studia la destinazione **GatherPackagesForPublishing** , si noterà che non contiene effettivamente alcuna attività. Contiene invece un solo gruppo di elementi che definisce tre elementi dinamici.

[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]

Questi elementi fanno riferimento ai pacchetti di distribuzione creati quando è stata eseguita la destinazione **BuildProjects** . Non è stato possibile definire questi elementi in modo statico nel file di progetto, perché i file a cui fanno riferimento gli elementi non esistono fino a quando non viene eseguita la destinazione **BuildProjects** . Al contrario, gli elementi devono essere definiti in modo dinamico all'interno di una destinazione che non viene richiamata finché non viene eseguita la destinazione **BuildProjects** .

Gli elementi non vengono usati all'interno di&#x2014;questa destinazione. questa destinazione compila semplicemente gli elementi e i metadati associati a ogni valore di elemento. Una volta elaborati questi elementi, l'elemento **PublishPackages** conterrà due valori, il percorso del file *ContactManager. Mvc. deploy. cmd* e il percorso del file *ContactManager. Service. deploy. cmd* . Distribuzione Web crea questi file come parte del pacchetto Web per ogni progetto e questi sono i file che devono essere richiamati nel server di destinazione per poter distribuire i pacchetti. Se si apre uno di questi file, si vedrà fondamentalmente un comando MSDeploy. exe con diversi valori di parametro specifici della compilazione.

L'elemento **DbPublishPackages** conterrà un singolo valore, ovvero il percorso del file *ContactManager. database. DeployManifest* .

> [!NOTE]
> Quando si compila un progetto di database e si utilizza lo stesso schema di un file di progetto MSBuild, viene generato un file con estensione DeployManifest. Contiene tutte le informazioni necessarie per distribuire un database, incluso il percorso dello schema del database (con estensione dbschema) e i dettagli di qualsiasi script di pre-distribuzione e post-distribuzione. Per ulteriori informazioni, vedere [una panoramica del compilazione e distribuzione di database](https://msdn.microsoft.com/library/aa833165.aspx).

Verranno fornite ulteriori informazioni sul modo in cui i pacchetti di distribuzione e i manifesti di distribuzione del database vengono creati e utilizzati per la [compilazione e la creazione](building-and-packaging-web-application-projects.md) di pacchetti di progetti di applicazioni Web e per la [distribuzione](deploying-database-projects.md)

### <a name="the-publishdbpackages-target"></a>Destinazione PublishDbPackages

In breve, la destinazione **PublishDbPackages** richiama l'utilità VSDBCMD per distribuire il database **ContactManager** in un ambiente di destinazione. La configurazione della distribuzione del database implica numerose decisioni e sfumature. per ulteriori informazioni, vedere [distribuzione di progetti di database](deploying-database-projects.md) e [personalizzazione delle distribuzioni di database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). In questo argomento verrà illustrato il modo in cui questa destinazione funziona effettivamente.

Si noti innanzitutto che il tag di apertura include un attributo **Outputs** .

[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]

Questo è un esempio di *batch di destinazione*. Nei file di progetto MSBuild, l'invio in batch è una tecnica per l'iterazione delle raccolte. Il valore dell'attributo **Outputs** , **"% (DbPublishPackages. Identity)"** , si riferisce alla proprietà metadati di **identità** dell'elenco di elementi **DbPublishPackages** . Questa notazione, **Outputs =% * * * (Item. ItemMetaDataName)* , viene tradotta come:

- Suddividere gli elementi in **DbPublishPackages** in batch di elementi che contengono lo stesso valore di metadati **Identity** .
- Eseguire la destinazione una volta per ogni batch.

> [!NOTE]
> **Identity** è uno dei [valori di metadati predefiniti](https://msdn.microsoft.com/library/ms164313.aspx) assegnati a ogni elemento al momento della creazione. Fa riferimento al valore dell'attributo **include** nell'elemento&#x2014; **Item** in altre parole, il percorso e il nome file dell'elemento.

In questo caso, poiché non deve essere mai presente più di un elemento con lo stesso percorso e nome file, stiamo essenzialmente lavorando con le dimensioni di un batch. La destinazione viene eseguita una volta per ogni pacchetto di database.

È possibile osservare una notazione simile nella proprietà **\_cmd** , che compila un comando VSDBCMD con le opzioni appropriate.

[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]

In questo caso, **% (DbPublishPackages. DatabaseConnectionString)** , **% (DbPublishPackages. TargetDatabase)** e **% (DbPublishPackages. FullPath)** fanno riferimento a valori di metadati della raccolta di elementi **DbPublishPackages** . La **\_proprietà cmd** viene utilizzata dall'attività **Exec** , che richiama il comando.

[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]

In seguito a questa notazione, l'attività **Exec** creerà batch in base a combinazioni univoche dei valori dei metadati **databaseConnectionString**, **TargetDatabase**e **FullPath** e l'attività verrà eseguita una volta per ogni batch. Questo è un esempio di suddivisione in *batch delle attività*. Tuttavia, poiché il batch a livello di destinazione ha già diviso la raccolta di elementi in batch a singolo elemento, l'attività **Exec** viene eseguita una sola volta per ogni iterazione della destinazione. In altre parole, questa attività richiama l'utilità VSDBCMD una volta per ogni pacchetto di database nella soluzione.

> [!NOTE]
> Per ulteriori informazioni sulla destinazione e l'invio in batch delle attività, vedere [batch](https://msdn.microsoft.com/library/ms171473.aspx)MSBuild, [metadati degli elementi in batch di destinazione](https://msdn.microsoft.com/library/ms228229.aspx)e [metadati degli elementi in batch di attività](https://msdn.microsoft.com/library/ms171474.aspx).

### <a name="the-publishwebpackages-target"></a>Destinazione PublishWebPackages

A questo punto, è stata richiamata la destinazione **BuildProjects** , che genera un pacchetto di distribuzione Web per ogni progetto nella soluzione di esempio. Ogni pacchetto è un file *deploy. cmd* , che contiene i comandi MSDeploy. exe necessari per distribuire il pacchetto nell'ambiente di destinazione e un file *separameters. XML* , che specifica i dettagli necessari dell'ambiente di destinazione. È stata anche richiamata la destinazione **GatherPackagesForPublishing** , che genera una raccolta di elementi che contiene i file di *deploy. cmd* a cui si è interessati. In pratica, la destinazione **PublishWebPackages** esegue queste funzioni:

- Modifica il file *Separameters. XML* per ogni pacchetto in modo da includere i dettagli corretti per l'ambiente di destinazione, usando l'attività **XmlPoke** .
- Richiama il file *deploy. cmd* per ogni pacchetto, usando le opzioni appropriate.

Proprio come la destinazione **PublishDbPackages** , la destinazione **PublishWebPackages** usa il batch di destinazione per garantire che la destinazione venga eseguita una volta per ogni pacchetto Web.

[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]

All'interno della destinazione, l'attività **Exec** viene utilizzata per eseguire il file *deploy. cmd* per ogni pacchetto Web.

[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]

Per ulteriori informazioni sulla configurazione della distribuzione di pacchetti Web, vedere [compilazione e creazione](building-and-packaging-web-application-projects.md)di pacchetti di progetti di applicazione Web.

## <a name="conclusion"></a>Conclusione

Questo argomento fornisce una procedura dettagliata sulla modalità di utilizzo dei file di progetto divisi per controllare il processo di compilazione e distribuzione dall'inizio alla fine della soluzione di esempio Contact Manager. L'uso di questo approccio consente di eseguire distribuzioni complesse su scala aziendale in un unico passaggio ripetibile, eseguendo semplicemente un file di comando specifico dell'ambiente.

## <a name="further-reading"></a>Ulteriori informazioni

Per un'introduzione più approfondita ai file di progetto e WPP, vedere [all'interno del Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) di Sayed Ibrahim Hashimi e William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Precedente](understanding-the-project-file.md)
> [Successivo](building-and-packaging-web-application-projects.md)
