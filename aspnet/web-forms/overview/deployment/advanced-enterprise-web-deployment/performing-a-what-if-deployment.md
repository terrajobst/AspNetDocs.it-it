---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Esecuzione di una distribuzione What If | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come eseguire le distribuzioni ' What If ' (o simulate) utilizzando lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web) e V...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 73a0e038cc0d4ebae0ffc8ed3fd2de4c9dad673c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628891"
---
# <a name="performing-a-what-if-deployment"></a>Esecuzione di una distribuzione di simulazione

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come eseguire le distribuzioni "simulazione" (o simulate) utilizzando lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web) e VSDBCMD. Ciò consente di determinare gli effetti della logica di distribuzione in un ambiente di destinazione specifico prima di distribuire effettivamente l'applicazione.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni è basato sull'approccio del file di progetto suddiviso descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione e distribuzione è&#x2014;controllato da due file di progetto, uno contenente le istruzioni di compilazione applicabili a tutti gli ambienti di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche per l'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Esecuzione di una distribuzione "What If" per i pacchetti Web

Distribuzione Web include funzionalità che consentono di eseguire le distribuzioni in modalità "simulazione" (o versione di valutazione). Quando si distribuiscono gli artefatti in modalità "simulazione", Distribuzione Web genera un file di log come se fosse stata eseguita la distribuzione, ma in realtà non cambia nulla nel server di destinazione. Esaminando il file di log è possibile comprendere l'effetto che la distribuzione avrà sul server di destinazione, in particolare:

- Che cosa viene aggiunto.
- Che cosa viene aggiornato.
- Che cosa viene eliminato.

Poiché una distribuzione di tipo "simulazione" non modifica nulla nel server di destinazione, non è sempre possibile prevedere se una distribuzione avrà esito positivo.

Come descritto in [distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md), è possibile distribuire i pacchetti web utilizzando distribuzione Web in&#x2014;due modi utilizzando l'utilità da riga di comando MSDeploy. exe direttamente oppure eseguendo il file *. deploy. cmd* generato dal processo di compilazione.

Se si usa direttamente MSDeploy. exe, è possibile eseguire una distribuzione di simulazione aggiungendo il flag **– whatIf** al comando. Ad esempio, per valutare cosa accadrebbe se il pacchetto ContactManager. Mvc. zip venisse distribuito in un ambiente di gestione temporanea, il comando MSDeploy sarà simile al seguente:

[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]

Quando si è soddisfatti dei risultati della distribuzione di simulazione, è possibile rimuovere il flag **– whatIf** per eseguire una distribuzione in tempo reale.

> [!NOTE]
> Per ulteriori informazioni sulle opzioni della riga di comando per MSDeploy. exe, vedere [distribuzione Web impostazioni dell'operazione](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Se si usa il file *. deploy. cmd* , è possibile eseguire una distribuzione di simulazione includendo il flag **/t** (modalità di valutazione) invece del flag **/y** ("Yes" o Update Mode) nel comando. Ad esempio, per valutare cosa accadrebbe se il pacchetto ContactManager. Mvc. zip venisse distribuito eseguendo il file *. deploy. cmd* , il comando dovrebbe essere simile al seguente:

[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]

Quando si è soddisfatti dei risultati della distribuzione della "modalità di valutazione", è possibile sostituire il flag **/t** con un flag **/y** per eseguire una distribuzione in tempo reale:

[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]

> [!NOTE]
> Per ulteriori informazioni sulle opzioni della riga di comando per i file *. deploy. cmd* , vedere [procedura: installare un pacchetto di distribuzione utilizzando il file Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx). Se si esegue il file *. deploy. cmd* senza specificare alcun flag, al prompt dei comandi verrà visualizzato un elenco di flag disponibili.

## <a name="performing-a-what-if-deployment-for-databases"></a>Esecuzione di una distribuzione "What If" per i database

In questa sezione si presuppone che si stia utilizzando l'utilità VSDBCMD per eseguire la distribuzione incrementale del database basata su Schema. Questo approccio viene descritto più dettagliatamente nella [distribuzione di progetti di database](../web-deployment-in-the-enterprise/deploying-database-projects.md). È consigliabile acquisire familiarità con questo argomento prima di applicare i concetti descritti qui.

Quando si usa VSDBCMD in modalità di **distribuzione** , è possibile usare il flag **/DD** (o **/DEPLOYTODATABASE**) per controllare se VSDBCMD distribuisce effettivamente il database o semplicemente genera uno script di distribuzione. Se si sta distribuendo un file con estensione dbschema, questo è il comportamento:

- Se si specifica **/DD +** o **/DD**, VSDBCMD genererà uno script di distribuzione e distribuirà il database.
- Se si specifica **/DD-** o si omette l'opzione, VSDBCMD genererà solo uno script di distribuzione.

> [!NOTE]
> Se si distribuisce un file con estensione deploymanifest anziché un file con estensione dbschema, il comportamento dell'opzione **/DD** è molto più complesso. In sostanza, VSDBCMD ignorerà il valore dell'opzione **/DD** se il file con estensione deploymanifest include un elemento **DeployToDatabase** con un valore **true**. La [distribuzione dei progetti di database](../web-deployment-in-the-enterprise/deploying-database-projects.md) descrive questo comportamento in modo completo.

Ad esempio, per generare uno script di distribuzione per il database **ContactManager** senza distribuire effettivamente il database, il comando VSDBCMD dovrebbe essere simile al seguente:

[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]

VSDBCMD è uno strumento di distribuzione differenziale del database e, di conseguenza, lo script di distribuzione viene generato dinamicamente per contenere tutti i comandi SQL necessari per aggiornare il database corrente, se presente, allo schema specificato. La revisione dello script di distribuzione è un modo utile per determinare l'effetto che la distribuzione avrà sul database corrente e i dati in esso contenuti. Ad esempio, potrebbe essere necessario determinare:

- Indica se verranno rimosse le tabelle esistenti e se questa operazione comporterà la perdita di dati.
- Indica se l'ordine delle operazioni comporta un rischio di perdita di dati, ad esempio se si stanno suddividendo o unendo tabelle.

Se si è soddisfatti dello script di distribuzione, è possibile ripetere il VSDBCMD con un flag **/DD +** per apportare le modifiche. In alternativa, è possibile modificare lo script di distribuzione per soddisfare i requisiti e quindi eseguirlo manualmente nel server di database.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrazione della funzionalità "What If" nei file di progetto personalizzati

Negli scenari di distribuzione più complessi, è consigliabile usare un file di progetto Microsoft Build Engine personalizzato (MSBuild) per incapsulare la logica di compilazione e distribuzione, come descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Ad esempio, nella soluzione di esempio [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , il file *Publish. proj* :

- Compila la soluzione.
- USA Distribuzione Web per creare il pacchetto e distribuire l'applicazione ContactManager. Mvc.
- USA Distribuzione Web per creare il pacchetto e distribuire l'applicazione ContactManager. Service.
- Distribuisce il database **ContactManager** .

Quando si integra la distribuzione di più pacchetti Web e/o database in un processo in un unico passaggio in questo modo, è anche possibile scegliere di eseguire l'intera distribuzione in una modalità di simulazione.

Il file *Publish. proj* illustra come è possibile eseguire questa operazione. Prima di tutto, è necessario creare una proprietà per archiviare il valore "What if":

[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]

In questo caso, è stata creata una proprietà denominata **whatIf** con un valore predefinito **false**. Gli utenti possono eseguire l'override di questo valore impostando la proprietà su **true** in un parametro della riga di comando, come si vedrà a breve.

La fase successiva consiste nel parametrizzare tutti i comandi Distribuzione Web e VSDBCMD in modo che i flag riflettano il valore della proprietà **whatIf** . Ad esempio, la destinazione successiva (ricavata dal file *Publish. proj* e semplificato) esegue il file *. deploy. cmd* per distribuire un pacchetto Web. Per impostazione predefinita, il comando include un'opzione **/Y** ("Yes" o "Update Mode"). Se **whatIf** è impostato su **true**, viene sostituito da un'opzione **/t** (versione di valutazione o modalità di simulazione).

[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]

Analogamente, la destinazione successiva usa l'utilità VSDBCMD per distribuire un database. Per impostazione predefinita, un'opzione **/DD** non è inclusa. Ciò significa che VSDBCMD genererà uno script di distribuzione, ma non distribuirà&#x2014;il database in altre parole, uno scenario di simulazione. Se la proprietà **whatIf** non è impostata su **true**, viene aggiunta un'opzione **/DD** e il database verrà distribuito da VSDBCMD.

[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]

È possibile usare lo stesso approccio per parametrizzare tutti i comandi pertinenti nel file di progetto. Quando si desidera eseguire una distribuzione di simulazione, è possibile fornire semplicemente un valore della proprietà **whatIf** dalla riga di comando:

[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]

In questo modo, è possibile eseguire una distribuzione di simulazione per tutti i componenti del progetto in un unico passaggio.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come eseguire le distribuzioni di tipo "simulazione" utilizzando Distribuzione Web, VSDBCMD e MSBuild. Una distribuzione "What if" consente di valutare l'impatto di una distribuzione proposta prima di apportare modifiche all'ambiente di destinazione.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla sintassi della riga di comando Distribuzione Web, vedere [distribuzione Web operation settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Per informazioni sulle opzioni della riga di comando quando si usa il file *. deploy. cmd* , vedere [procedura: installare un pacchetto di distribuzione usando il file Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx). Per istruzioni sulla sintassi della riga di comando VSDBCMD, vedere [riferimenti alla riga di comando per VSDBCMD. EXE (distribuzione e importazione dello schema)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Precedente](advanced-enterprise-web-deployment.md)
> [Successivo](customizing-database-deployments-for-multiple-environments.md)
