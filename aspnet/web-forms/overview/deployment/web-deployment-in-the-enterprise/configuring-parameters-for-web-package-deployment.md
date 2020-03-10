---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configurazione dei parametri per la distribuzione di pacchetti Web | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come impostare i valori dei parametri, ad esempio i nomi delle applicazioni Web Internet Information Services (IIS), le stringhe di connessione e gli endpoint di servizio,...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: f04ace98d81a33053b10cab7e40dbd75a6c0992c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544954"
---
# <a name="configuring-parameters-for-web-package-deployment"></a>Configurazione dei parametri per la distribuzione di pacchetti Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come impostare i valori dei parametri, ad esempio i nomi delle applicazioni Web Internet Information Services (IIS), le stringhe di connessione e gli endpoint di servizio, quando si distribuisce un pacchetto Web in un server Web IIS remoto.

Quando si compila un progetto di applicazione Web, il processo di compilazione e creazione di pacchetti genera tre file di chiave:

- Un file con estensione *zip [nome progetto]* . Si tratta del pacchetto di distribuzione Web per il progetto di applicazione Web. Questo pacchetto contiene tutti gli assembly, i file, gli script di database e le risorse necessarie per ricreare l'applicazione Web in un server Web IIS remoto.
- Un file *[nome progetto]. deploy. cmd* . Contiene un set di comandi Distribuzione Web con parametri (MSDeploy. exe) che pubblicano il pacchetto di distribuzione Web in un server Web IIS remoto.
- *[Nome progetto]. File separameters. XML* . In questo modo viene fornito un set di valori di parametro al comando MSDeploy. exe. È possibile aggiornare i valori in questo file e passarli a Distribuzione Web come parametro della riga di comando quando si distribuisce il pacchetto Web.

> [!NOTE]
> Per ulteriori informazioni sul processo di compilazione e creazione di pacchetti, vedere [compilazione e creazione di pacchetti di progetti di applicazione Web](building-and-packaging-web-application-projects.md).

Il file *Separameters. XML* viene generato in modo dinamico dal file di progetto dell'applicazione Web e da eventuali file di configurazione all'interno del progetto. Quando si compila e si crea il pacchetto del progetto, la pipeline di pubblicazione sul Web (WPP) rileva automaticamente molte delle variabili che probabilmente cambiano tra gli ambienti di distribuzione, ad esempio l'applicazione Web IIS di destinazione e le stringhe di connessione del database. Questi valori vengono parametrizzati automaticamente nel pacchetto di distribuzione Web e aggiunti al file *Separameters. XML* . Se ad esempio si aggiunge una stringa di connessione al file *Web. config* nel progetto di applicazione Web, il processo di compilazione rileverà questa modifica e aggiungerà una voce al file *separameters. XML* di conseguenza.

In numerosi casi, la parametrizzazione automatica sarà sufficiente. Tuttavia, se gli utenti devono variare altre impostazioni tra gli ambienti di distribuzione, ad esempio le impostazioni dell'applicazione o gli URL degli endpoint di servizio, è necessario indicare a WPP di parametrizzare questi valori nel pacchetto di distribuzione e aggiungere le voci corrispondenti al file *Separameters. XML* . Le sezioni seguenti illustrano come eseguire questa operazione.

### <a name="automatic-parameterization"></a>Parametrizzazione automatica

Quando si compila e si crea il pacchetto di un'applicazione Web, il WPP parametrizza automaticamente questi elementi:

- Il percorso e il nome dell'applicazione Web IIS di destinazione.
- Tutte le stringhe di connessione nel file *Web. config* .
- Stringhe di connessione per tutti i database aggiunti alla scheda **pubblicazione/pubblicazione pacchetto SQL** nelle pagine delle proprietà del progetto.

Se ad esempio si compila e si crea il pacchetto della soluzione di esempio [Contact Manager](the-contact-manager-solution.md) senza toccare il processo di parametrizzazione in alcun modo, il WPP genera il file *ContactManager. Mvc. separameters. XML* :

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]

In questo caso:

- Il parametro del **nome dell'applicazione Web IIS** è il percorso IIS in cui si desidera distribuire l'applicazione Web. Il valore predefinito viene tratto dalla pagina **Web del pacchetto o della pubblicazione** nelle pagine delle proprietà del progetto.
- Il parametro della **stringa di connessione ApplicationServices-Web. config** è stato generato da un elemento **connectionStrings/add** nel file *Web. config* . Rappresenta la stringa di connessione che l'applicazione deve utilizzare per contattare il database delle appartenenze. Il valore fornito qui verrà sostituito con il file *Web. config* distribuito. Il valore predefinito viene ricavato dal file *Web. config* pre-distribuzione.

Anche WPP parametrizza queste proprietà nel pacchetto di distribuzione che genera. Quando si installa il pacchetto di distribuzione, è possibile specificare i valori per queste proprietà. Se il pacchetto viene installato manualmente tramite Gestione IIS, come descritto in [installazione manuale di pacchetti Web](manually-installing-web-packages.md), l'installazione guidata richiede di specificare i valori per tutti i parametri. Se si installa il pacchetto in modalità remota utilizzando il file *. deploy. cmd* , come descritto in [Deploying Web packages](deploying-web-packages.md), distribuzione Web guarderà il file *separameters. XML* per specificare i valori dei parametri. È possibile modificare i valori nel file *Separameters. XML* manualmente oppure è possibile personalizzare il file come parte di un processo di compilazione e distribuzione automatizzato. Questo processo è descritto in modo più dettagliato più avanti in questo argomento.

### <a name="custom-parameterization"></a>Parametrizzazione personalizzata

Negli scenari di distribuzione più complessi è spesso necessario parametrizzare proprietà aggiuntive prima di distribuire il progetto. In generale, è necessario parametrizzare le proprietà e le impostazioni che variano tra gli ambienti di destinazione. Questi possono includere:

- Endpoint del servizio nel file *Web. config* .
- Impostazioni dell'applicazione nel file *Web. config* .
- Qualsiasi altra proprietà dichiarativa che si desidera richiedere agli utenti di specificare.

Il modo più semplice per parametrizzare queste proprietà consiste nell'aggiungere un file *Parameters. XML* alla cartella radice del progetto di applicazione Web. Ad esempio, nella soluzione Contact Manager il progetto ContactManager. MVC include un file *Parameters. XML* nella cartella radice.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Se si apre questo file, si noterà che contiene una voce di **parametro** singolo. La voce utilizza una query XPath (XML Path Language) per individuare e parametrizzare l'URL dell'endpoint del servizio ContactService Windows Communication Foundation (WCF) nel file *Web. config* .

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]

Oltre a parametrizzazione l'URL dell'endpoint nel pacchetto di distribuzione, WPP aggiunge anche una voce corrispondente al file *Separameters. XML* che viene generato insieme al pacchetto di distribuzione.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]

Se il pacchetto di distribuzione viene installato manualmente, gestione IIS richiederà l'indirizzo dell'endpoint del servizio insieme alle proprietà parametrizzate automaticamente. Se si installa il pacchetto di distribuzione eseguendo il file *. deploy. cmd* , è possibile modificare il file *separameters. XML* per fornire un valore per l'indirizzo dell'endpoint del servizio insieme ai valori per le proprietà che sono state parametrizzate automaticamente.

Per informazioni dettagliate su come creare un file *Parameters. XML* , vedere [procedura: usare i parametri per configurare le impostazioni di distribuzione durante l'installazione di un pacchetto](https://msdn.microsoft.com/library/ff398068.aspx). La procedura denominata **per l'utilizzo dei parametri di distribuzione per le impostazioni del file Web. config** include istruzioni dettagliate.

## <a name="modifying-the-setparametersxml-file"></a>Modifica del file separameters. XML

Se si prevede di distribuire il pacchetto dell'applicazione Web&#x2014;manualmente eseguendo il file *. deploy. cmd* o eseguendo MSDeploy. exe dalla riga&#x2014;di comando, non c'è nulla da arrestare per modificare manualmente il file *separameters. XML* prima della distribuzione. Tuttavia, se si lavora su una soluzione su scala aziendale, potrebbe essere necessario distribuire un pacchetto di applicazione Web come parte di un processo di compilazione e distribuzione automatizzato più ampio. In questo scenario è necessario il Microsoft Build Engine (MSBuild) per modificare il file *Separameters. XML* . A tale scopo, è possibile usare l'attività MSBuild **XmlPoke** .

Questo processo è illustrato nella [soluzione di esempio Contact Manager](the-contact-manager-solution.md) . Gli esempi di codice seguenti sono stati modificati in modo da visualizzare solo i dettagli relativi a questo esempio.

> [!NOTE]
> Per una panoramica più ampia del modello di file di progetto nella soluzione di esempio e un'introduzione ai file di progetto personalizzati in generale, vedere [informazioni sul file di progetto](understanding-the-project-file.md) e [informazioni sul processo di compilazione](understanding-the-build-process.md).

In primo luogo, i valori dei parametri di interesse vengono definiti come proprietà nel file di progetto specifico dell'ambiente (ad esempio, *env-dev. proj*).

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]

> [!NOTE]
> Per istruzioni su come personalizzare i file di progetto specifici dell'ambiente per gli ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Successivamente, il file *Publish. proj* importa queste proprietà. Poiché ogni *file Separameters. XML* è associato a un file *. deploy. cmd* e si desidera che il file di progetto richiami ogni file *. deploy. cmd* , il file di progetto crea un *elemento* MSBuild per ogni file *. deploy. cmd* e definisce le proprietà di interesse come *metadati dell'elemento*.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]

In questo caso:

- Il valore dei metadati **ParametersXml** indica il percorso del file *separameters. XML* .
- Il valore **IisWebAppName** è il percorso IIS in cui si desidera distribuire l'applicazione Web.
- Il valore **MembershipDBConnectionString** è la stringa di connessione per il database di appartenenza e il valore **MembershipDBConnectionName** è l'attributo **Name** del parametro corrispondente nel file *separameters. XML* .
- Il valore **ServiceEndpointValue** è l'indirizzo endpoint per il servizio WCF nel server di destinazione e il valore **ServiceEndpointParamName** è l'attributo Name del parametro corrispondente nel file *separameters. XML* .

Infine, nel file *Publish. proj* la destinazione **PublishWebPackages** usa l'attività **XmlPoke** per modificare questi valori nel file *separameters. XML* .

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]

Si noterà che ogni attività **XmlPoke** specifica quattro valori di attributo:

- L'attributo **XmlInputPath** indica all'attività dove trovare il file che si desidera modificare.
- L'attributo **query** è una query XPath che identifica il nodo XML che si desidera modificare.
- L'attributo **value** è il nuovo valore che si desidera inserire nel nodo XML selezionato.
- L'attributo **Condition** è costituito dai criteri in cui l'attività deve essere eseguita o meno. In questi casi, la condizione garantisce che non si tenti di inserire un valore null o vuoto nel file *Separameters. XML* .

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto il ruolo del file *Separameters. XML* e viene illustrato come viene generato quando si compila un progetto di applicazione Web. È stato illustrato come parametrizzare impostazioni aggiuntive aggiungendo un file *Parameters. XML* al progetto. Viene anche descritto come modificare il file *Separameters. XML* come parte di un processo di compilazione automatizzato più grande, usando l'attività **XmlPoke** nei file di progetto.

Nell'argomento successivo, [distribuzione di pacchetti Web](deploying-web-packages.md), viene descritto come è possibile distribuire un pacchetto Web eseguendo il file *. deploy. cmd* o usando direttamente i comandi MSDeploy. exe. In entrambi i casi, è possibile specificare il file *Separameters. XML* come parametro di distribuzione.

## <a name="further-reading"></a>Ulteriori informazioni

Per informazioni su come creare pacchetti Web, vedere [compilazione e creazione di pacchetti di progetti di applicazioni Web](building-and-packaging-web-application-projects.md). Per istruzioni su come distribuire effettivamente un pacchetto Web, vedere [distribuzione di pacchetti Web](deploying-web-packages.md). Per una procedura dettagliata su come creare un file *Parameters. XML* , vedere [procedura: usare i parametri per configurare le impostazioni di distribuzione durante l'installazione di un pacchetto](https://msdn.microsoft.com/library/ff398068.aspx).

Per informazioni più generali sulla parametrizzazione in Distribuzione Web, vedere [distribuzione Web parametrizzazione in azione](https://go.microsoft.com/?linkid=9805119) (post di Blog).

> [!div class="step-by-step"]
> [Precedente](building-and-packaging-web-application-projects.md)
> [Successivo](deploying-web-packages.md)
