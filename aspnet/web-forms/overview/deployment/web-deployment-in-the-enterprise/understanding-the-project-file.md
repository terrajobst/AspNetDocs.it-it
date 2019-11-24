---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Informazioni sul file di progetto | Microsoft Docs
author: jrjlee
description: I file di progetto Microsoft Build Engine (MSBuild) si trovano nella parte centrale del processo di compilazione e distribuzione. Questo argomento inizia con una panoramica concettuale di MSBuild...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 419fe51aaf65bddcc2c50380f099f842a8d9439c
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445689"
---
# <a name="understanding-the-project-file"></a>Informazioni sul file di progetto

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> I file di progetto Microsoft Build Engine (MSBuild) si trovano nella parte centrale del processo di compilazione e distribuzione. Questo argomento inizia con una panoramica concettuale di MSBuild e del file di progetto. Descrive i componenti principali che verranno usati quando si lavora con i file di progetto e funziona con un esempio di come è possibile usare i file di progetto per distribuire applicazioni reali.
> 
> Cosa si apprenderà:
> 
> - Modalità di utilizzo dei file di progetto MSBuild per la compilazione di progetti.
> - Il modo in cui MSBuild si integra con le tecnologie di distribuzione, ad esempio lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web).
> - Come comprendere i componenti chiave di un file di progetto.
> - Come è possibile utilizzare i file di progetto per compilare e distribuire applicazioni complesse.

## <a name="msbuild-and-the-project-file"></a>MSBuild e il file di progetto

Quando si creano e si compilano soluzioni in Visual Studio, Visual Studio USA MSBuild per compilare ogni progetto nella soluzione. Ogni progetto di Visual Studio include un file di progetto MSBuild, con un'estensione di file che riflette il&#x2014;tipo di progetto, C# ad esempio un progetto (csproj), un progetto Visual Basic.NET (vbproj) o un progetto di database (. dbproj). Per compilare un progetto, MSBuild deve elaborare il file di progetto associato al progetto. Il file di progetto è un documento XML contenente tutte le informazioni e le istruzioni necessarie a MSBuild per compilare il progetto, ad esempio il contenuto da includere, i requisiti della piattaforma, le informazioni sul controllo delle versioni, le impostazioni del server Web o del server di database e il attività che devono essere eseguite.

I file di progetto MSBuild sono basati sulla [XML schema MSBuild](/visualstudio/msbuild/msbuild-project-file-schema-reference)e, di conseguenza, il processo di compilazione è completamente aperto e trasparente. Inoltre, non è necessario installare Visual Studio per usare il motore&#x2014;MSBuild. il file eseguibile di MSBuild. exe fa parte della .NET Framework ed è possibile eseguirlo da un prompt dei comandi. Gli sviluppatori possono creare file di progetto MSBuild personalizzati, usando il XML Schema MSBuild, per imporre un controllo sofisticato e con granularità fine sulla modalità di compilazione e distribuzione dei progetti. Questi file di progetto personalizzati funzionano esattamente allo stesso modo dei file di progetto generati automaticamente da Visual Studio.

> [!NOTE]
> È anche possibile usare i file di progetto MSBuild con il servizio Team Build in Team Foundation Server (TFS). Ad esempio, è possibile usare i file di progetto in scenari di integrazione continua (CI) per automatizzare la distribuzione in un ambiente di test quando viene archiviato nuovo codice. Per altre informazioni, vedere [configurazione di Team Foundation Server per la distribuzione Web automatizzata](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).

### <a name="project-file-naming-conventions"></a>Convenzioni di denominazione dei file di progetto

Quando si creano file di progetto personalizzati, è possibile usare qualsiasi estensione di file. Tuttavia, per semplificare la comprensione delle soluzioni, è consigliabile usare le convenzioni comuni seguenti:

- Utilizzare l'estensione proj quando si crea un file di progetto che compila i progetti.
- Utilizzare l'estensione. targets quando si crea un file di progetto riutilizzabile per l'importazione in altri file di progetto. I file con estensione targets in genere non compilano nulla, ma contengono semplicemente istruzioni che è possibile importare nei file con estensione proj.

### <a name="integration-with-deployment-technologies"></a>Integrazione con le tecnologie di distribuzione

Se sono stati usati progetti di applicazione Web in Visual Studio 2010, come le applicazioni Web ASP.NET e le applicazioni Web ASP.NET MVC, si saprà che questi progetti includono il supporto incorporato per la creazione di pacchetti e la distribuzione dell'applicazione Web in un ambiente di destinazione. Le pagine delle **Proprietà** per questi progetti includono le schede **pubblicazione/pubblicazione** **del pacchetto e** del Web, che è possibile usare per configurare il pacchetto e la distribuzione dei componenti dell'applicazione. Viene visualizzata la scheda **pubblicazione/pubblicazione del pacchetto Web** :

![](understanding-the-project-file/_static/image1.png)

La tecnologia sottostante di queste funzionalità è nota come Web Publishing pipeline (WPP). Il WPP in sostanza unisce MSBuild e [distribuzione Web](https://go.microsoft.com/?linkid=9805122) per offrire un processo completo di compilazione, creazione di pacchetti e distribuzione per le applicazioni Web.

La novità è che è possibile sfruttare i punti di integrazione forniti da WPP quando si creano file di progetto personalizzati per i progetti Web. È possibile includere nel file di progetto le istruzioni per la distribuzione, che consentono di compilare i progetti, creare pacchetti di distribuzione Web e installare i pacchetti in server remoti tramite un singolo file di progetto e una singola chiamata a MSBuild. È anche possibile chiamare qualsiasi altro file eseguibile come parte del processo di compilazione. È ad esempio possibile eseguire lo strumento da riga di comando VSDBCMD. exe per distribuire un database da un file di schema. Nel corso di questo argomento, si vedrà come sfruttare queste funzionalità per soddisfare i requisiti degli scenari di distribuzione aziendali.

> [!NOTE]
> Per altre informazioni sul funzionamento del processo di distribuzione dell'applicazione Web, vedere [Panoramica della distribuzione del progetto di applicazione web ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx).

## <a name="the-anatomy-of-a-project-file"></a>Anatomia di un file di progetto

Prima di esaminare il processo di compilazione in modo più dettagliato, è opportuno richiedere alcuni istanti per acquisire familiarità con la struttura di base di un file di progetto MSBuild. In questa sezione viene fornita una panoramica degli elementi più comuni che si verificheranno quando si esamina, si modifica o si crea un file di progetto. In particolare, si apprenderà come:

- Come usare le *Proprietà* per gestire le variabili per il processo di compilazione.
- Come usare *gli elementi* per identificare gli input per il processo di compilazione, ad esempio i file di codice.
- Come usare le *destinazioni* e le *attività* per fornire istruzioni di esecuzione a MSBuild, usando le *proprietà* e *gli elementi* definiti in un'altra posizione nel file di progetto.

Viene visualizzata la relazione tra gli elementi chiave in un file di progetto MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Elemento Project

L'elemento [Project](https://msdn.microsoft.com/library/bcxfsh87.aspx) è l'elemento radice di ogni file di progetto. Oltre a identificare i XML Schema per il file di progetto, l'elemento **Project** può includere attributi per specificare i punti di ingresso per il processo di compilazione. Nella [soluzione di esempio Contact Manager](the-contact-manager-solution.md), ad esempio, il file *Publish. proj* specifica che la compilazione deve iniziare chiamando la destinazione denominata **FullPublish**.

[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]

### <a name="properties-and-conditions"></a>Proprietà e condizioni

Un file di progetto in genere deve fornire diverse informazioni per compilare e distribuire i progetti in modo corretto. Queste informazioni possono includere nomi di server, stringhe di connessione, credenziali, configurazioni di compilazione, percorsi di file di origine e di destinazione e qualsiasi altra informazione che si desidera includere per supportare la personalizzazione. In un file di progetto, le proprietà devono essere definite all'interno di un elemento [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) . Le proprietà di MSBuild sono costituite da coppie chiave-valore. All'interno dell'elemento **PropertyGroup** , il nome dell'elemento definisce la chiave della proprietà e il contenuto dell'elemento definisce il valore della proprietà. Ad esempio, è possibile definire proprietà denominate **ServerName** e **ConnectionString** per archiviare un nome di server statico e una stringa di connessione.

[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]

Per recuperare un valore di proprietà, utilizzare il formato *$ (propertyName)* . Ad esempio, per recuperare il valore della proprietà **ServerName** , digitare:

[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]

> [!NOTE]
> Verranno visualizzati esempi di come e quando utilizzare i valori delle proprietà più avanti in questo argomento.

L'incorporamento di informazioni come proprietà statiche in un file di progetto non è sempre l'approccio ideale per la gestione del processo di compilazione. In numerosi scenari, è opportuno ottenere le informazioni da altre origini o consentire all'utente di fornire le informazioni dal prompt dei comandi. MSBuild consente di specificare qualsiasi valore di proprietà come parametro della riga di comando. Ad esempio, l'utente può specificare un valore per **nomeserver** quando esegue MSBuild. exe dalla riga di comando.

[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]

> [!NOTE]
> Per altre informazioni sugli argomenti e sulle opzioni che è possibile usare con MSBuild. exe, vedere [riferimenti alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

È possibile utilizzare la stessa sintassi di proprietà per ottenere i valori delle variabili di ambiente e delle proprietà predefinite del progetto. Molte proprietà di uso comune sono definite per l'utente e possono essere usate nei file di progetto includendo il nome del parametro pertinente. &#x2014;Ad esempio, per recuperare la piattaforma del progetto corrente, ad esempio **x86** o **AnyCpu**&#x2014;, è possibile includere il riferimento alla proprietà **$ (Platform)** nel file di progetto. Per ulteriori informazioni, vedere [macro per comandi e proprietà di compilazione](https://msdn.microsoft.com/library/c02as0cs.aspx), [proprietà del progetto MSBuild comuni](https://msdn.microsoft.com/library/bb629394.aspx)e [proprietà riservate](https://msdn.microsoft.com/library/ms164309.aspx).

Le proprietà vengono spesso utilizzate in combinazione con *le condizioni*. La maggior parte degli elementi MSBuild supporta l'attributo **Condition** , che consente di specificare i criteri in base ai quali MSBuild deve valutare l'elemento. Si consideri ad esempio questa definizione di proprietà:

[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]

Quando MSBuild elabora questa definizione di proprietà, verifica innanzitutto se è disponibile un valore della proprietà **$ (OutputRoot)** . Se il valore della proprietà è&#x2014;vuoto in altre parole, l'utente non ha specificato un valore per&#x2014;questa proprietà, la condizione restituisce **true** e il valore della proprietà è impostato su **. \Publish\Out**. Se l'utente ha specificato un valore per questa proprietà, la condizione restituisce **false** e il valore della proprietà statica non viene utilizzato.

Per altre informazioni sui diversi modi in cui è possibile specificare le condizioni, vedere [condizioni di MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Elementi e gruppi di elementi

Uno dei ruoli importanti del file di progetto consiste nel definire gli input per il processo di compilazione. In genere, questi input sono&#x2014;file di codice file, file di configurazione, file di comando e qualsiasi altro file che è necessario elaborare o copiare come parte del processo di compilazione. Nello schema del progetto MSBuild questi input sono rappresentati da elementi [elemento](https://msdn.microsoft.com/library/ms164283.aspx) . In un file di progetto, gli elementi devono essere definiti all'interno di un elemento [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) . Analogamente agli elementi della **Proprietà** , è possibile assegnare un nome a un elemento **Item** . Tuttavia, è necessario specificare un attributo **include** per identificare il file o il carattere jolly rappresentato dall'elemento.

[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]

Specificando più elementi **Item** con lo stesso nome, si crea effettivamente un elenco denominato di risorse. Un modo efficace per verificare questo aspetto è esaminare uno dei file di progetto creati da Visual Studio. Ad esempio, il file *ContactManager. Mvc. csproj* nella soluzione di esempio include molti gruppi **di elementi,** ognuno con diversi elementi con nome identico.

[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]

In questo&#x2014;modo, il file di progetto indica a MSBuild di costruire elenchi di file che devono essere elaborati nello stesso modo in cui l'elenco di **riferimenti** include gli assembly che devono essere presenti per una compilazione corretta, l'elenco di **compilazione** include i file di codice che devono essere compilati e l'elenco di **contenuto** include le risorse che devono essere copiate senza modifiche. Si esaminerà il modo in cui il processo di compilazione fa riferimento a questi elementi e li usa più avanti in questo argomento.

Gli elementi elemento possono includere anche elementi figlio [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) . Si tratta di coppie chiave-valore definite dall'utente e rappresentano essenzialmente proprietà specifiche di tale elemento. Ad esempio, una gran parte degli elementi di **compilazione** nel file di progetto include gli elementi figlio **DependentUpon** .

[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]

> [!NOTE]
> Oltre ai metadati degli elementi creati dall'utente, a tutti gli elementi vengono assegnati diversi metadati comuni durante la creazione. Per altre informazioni, vedere [Metadati noti degli elementi](https://msdn.microsoft.com/library/ms164313.aspx).

È possibile creare elementi **ItemGroup** all'interno dell'elemento di **progetto** a livello di radice o in specifici elementi di **destinazione** . Gli elementi **ItemGroup** supportano anche attributi di **condizione** , che consentono di adattare gli input al processo di compilazione in base a condizioni come la configurazione o la piattaforma del progetto.

### <a name="targets-and-tasks"></a>Destinazioni e attività

Nello schema MSBuild, un elemento [Task](https://msdn.microsoft.com/library/77f2hx1s.aspx) rappresenta una singola istruzione di compilazione (o attività). MSBuild include numerose attività predefinite. Ad esempio:

- L'attività **Copy** copia i file in un nuovo percorso.
- L'attività **CSC** richiama il compilatore visivo C# .
- L'attività **vbc** richiama il compilatore Visual Basic.
- L'attività **Exec** esegue un programma specifico.
- L'attività **Message** scrive un messaggio in un logger.

> [!NOTE]
> Per informazioni dettagliate sulle attività disponibili predefinite, vedere informazioni di riferimento sulle attività di [MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Per altre informazioni sulle attività, inclusa la creazione di attività personalizzate, vedere [attività MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).

Le attività devono essere sempre contenute negli elementi di [destinazione](https://msdn.microsoft.com/library/t50z2hka.aspx) . Un elemento di **destinazione** è un set di una o più attività eseguite in sequenza e un file di progetto può contenere più destinazioni. Quando si desidera eseguire un'attività o un set di attività, è necessario richiamare la destinazione che li contiene. Si supponga, ad esempio, di disporre di un file di progetto semplice che registra un messaggio.

[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]

È possibile richiamare la destinazione dalla riga di comando usando l'opzione **/t** per specificare la destinazione.

[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]

In alternativa, è possibile aggiungere un attributo **DefaultTargets** all'elemento **Project** per specificare le destinazioni che si desidera richiamare.

[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]

In questo caso, non è necessario specificare la destinazione dalla riga di comando. È possibile specificare semplicemente il file di progetto e MSBuild richiamerà la destinazione **FullPublish** .

[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]

Sia le destinazioni che le attività possono includere attributi di **condizione** . Di conseguenza, è possibile scegliere di omettere intere destinazioni o singole attività se vengono soddisfatte determinate condizioni.

In generale, quando si creano attività e destinazioni utili, è necessario fare riferimento alle proprietà e agli elementi definiti in un'altra posizione nel file di progetto:

- Per usare un valore di proprietà, digitare **$ (***PropertyName***)** , dove *PropertyName* è il nome dell'elemento **Property** o il nome del parametro.
- Per usare un elemento, digitare **@ (***ItemName***)** , dove *ItemName* è il nome dell'elemento **Item** .

> [!NOTE]
> Tenere presente che se si creano più elementi con lo stesso nome, si compila un elenco. Al contrario, se si creano più proprietà con lo stesso nome, l'ultimo valore di proprietà fornito sovrascriverà eventuali proprietà precedenti con lo stesso&#x2014;nome, una proprietà può contenere solo un valore singolo.

Ad esempio, nel file *Publish. proj* della soluzione di esempio esaminare la destinazione **BuildProjects** .

[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]

In questo esempio è possibile osservare questi punti chiave:

- Se il parametro **BuildingInTeamBuild** è specificato e il valore è **true**, nessuna delle attività all'interno di questa destinazione verrà eseguita.
- La destinazione contiene una singola istanza dell'attività [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) . Questa attività consente di compilare altri progetti MSBuild.
- L'elemento **ProjectsToBuild** viene passato all'attività. Questo elemento potrebbe rappresentare un elenco di file di progetto o di soluzione, tutti definiti da elementi elemento **ProjectsToBuild** all'interno di un gruppo di elementi. In questo caso, l'elemento **ProjectsToBuild** fa riferimento a un singolo file di soluzione.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- I valori delle proprietà passati all'attività **MSBuild** includono i parametri denominati **OutputRoot** e **Configuration**. Sono impostati sui valori dei parametri, se specificati, o i valori di proprietà statiche in caso contrario.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

È anche possibile vedere che l'attività **MSBuild** richiama una destinazione denominata **Build**. Questa è una delle diverse destinazioni predefinite che sono ampiamente usate nei file di progetto di Visual Studio e sono disponibili nei file di progetto personalizzati, ad esempio **compilazione**, **pulizia**, **ricompilazione**e **pubblicazione**. Verranno fornite altre informazioni sull'utilizzo di destinazioni e attività per controllare il processo di compilazione e sull'attività **MSBuild** in particolare, più avanti in questo argomento.

> [!NOTE]
> Per altre informazioni sulle destinazioni, vedere [destinazioni di MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).

## <a name="splitting-project-files-to-support-multiple-environments"></a>Suddivisione dei file di progetto per supportare più ambienti

Si supponga di voler essere in grado di distribuire una soluzione in più ambienti, ad esempio server di prova, piattaforme di staging e ambienti di produzione. La configurazione può variare sostanzialmente tra questi ambienti&#x2014;non solo in termini di nomi di server, stringhe di connessione e così via, ma anche in termini di credenziali, impostazioni di sicurezza e molti altri fattori. Se è necessario eseguire questa operazione regolarmente, non è molto opportuno modificare più proprietà nel file di progetto ogni volta che si cambia l'ambiente di destinazione. Né è una soluzione ideale per richiedere un elenco infinito di valori di proprietà da fornire al processo di compilazione.

Fortunatamente esiste un'alternativa. MSBuild consente di suddividere la configurazione della build tra più file di progetto. Per verificarne il funzionamento, nella soluzione di esempio si noti che sono presenti due file di progetto personalizzati:

- *Publish. proj*, che contiene proprietà, elementi e destinazioni comuni a tutti gli ambienti.
- *Env-dev. proj*, che contiene le proprietà specifiche di un ambiente di sviluppo.

Si noti ora che il file *Publish. proj* include un elemento [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) immediatamente sotto il tag del **progetto** Opening.

[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]

L'elemento **Import** viene usato per importare il contenuto di un altro file di progetto MSBuild nel file di progetto MSBuild corrente. In questo caso, il parametro **TargetEnvPropsFile** fornisce il nome del file di progetto che si vuole importare. Quando si esegue MSBuild, è possibile specificare un valore per questo parametro.

[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]

In questo modo, i contenuti dei due file vengono uniti in un unico file di progetto. Utilizzando questo approccio, è possibile creare un file di progetto contenente la configurazione della build universale e più file di progetto supplementari contenenti proprietà specifiche dell'ambiente. Di conseguenza, è sufficiente eseguire un comando con un valore di parametro diverso, che consente di distribuire la soluzione in un ambiente diverso.

![](understanding-the-project-file/_static/image3.png)

La suddivisione dei file di progetto in questo modo è una procedura consigliata da seguire. Consente agli sviluppatori di eseguire la distribuzione in più ambienti eseguendo un unico comando, evitando la duplicazione delle proprietà di compilazione universale tra più file di progetto.

> [!NOTE]
> Per istruzioni su come personalizzare i file di progetto specifici dell'ambiente per gli ambienti server, vedere [configurazione delle proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="conclusion"></a>Conclusione

Questo argomento fornisce un'introduzione generale ai file di progetto MSBuild e spiega come creare file di progetto personalizzati per controllare il processo di compilazione. È stato inoltre introdotto il concetto di suddivisione dei file di progetto nelle istruzioni di compilazione universale e delle proprietà di compilazione specifiche dell'ambiente, per semplificare la compilazione e la distribuzione di progetti in più destinazioni.

L'argomento successivo, che [comprende il processo di compilazione](understanding-the-build-process.md), fornisce informazioni più approfondite su come usare i file di progetto per controllare la compilazione e la distribuzione seguendo la distribuzione di una soluzione con un livello di complessità realistico.

## <a name="further-reading"></a>Ulteriori informazioni

Per un'introduzione più approfondita ai file di progetto e WPP, vedere [all'interno del Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) di Sayed Ibrahim Hashimi e William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Precedente](setting-up-the-contact-manager-solution.md)
> [Successivo](understanding-the-build-process.md)
