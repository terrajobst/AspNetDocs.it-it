---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Creazione ed esecuzione di un file di comando di distribuzione | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come compilare un file di comando che consente di eseguire una distribuzione utilizzando i file di progetto Microsoft Build Engine (MSBuild) come un singolo passaggio, re...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: f1477ff423e4898385066a35b42503f3c70dcc68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634309"
---
# <a name="creating-and-running-a-deployment-command-file"></a>Creazione ed esecuzione di un file di comando per la distribuzione

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come compilare un file di comando che consente di eseguire una distribuzione utilizzando i file di progetto di Microsoft Build Engine (MSBuild) come processo ripetibile a singolo passaggio.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014;soluzione [Contact Manager](the-contact-manager-solution.md) per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul processo di compilazione](understanding-the-build-process.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente le istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="process-overview"></a>Panoramica del processo

In questo argomento si apprenderà come creare ed eseguire un file di comando che usa questi file di progetto per eseguire una distribuzione ripetibile nell'ambiente di destinazione. In pratica, il file di comando deve semplicemente contenere un comando MSBuild che:

- Indica a MSBuild di eseguire il file con *estensione proj di pubblicazione* indipendente dall'ambiente.
- Indica al file *Publish. proj* il file contenente le impostazioni del progetto specifiche dell'ambiente e il percorso in cui trovarlo.

## <a name="create-an-msbuild-command"></a>Creare un comando MSBuild

Come descritto in [informazioni sul processo di compilazione](understanding-the-build-process.md), il file&#x2014;di progetto specifico dell'ambiente, ad esempio, *env-dev. proj*&#x2014;è progettato per essere importato nel file *Publish. proj* indipendente dall'ambiente in fase di compilazione. Insieme, questi due file forniscono un set completo di istruzioni che indicano a MSBuild come compilare e distribuire la soluzione.

Il file *Publish. proj* utilizza un elemento **Import** per importare il file di progetto specifico dell'ambiente.

[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]

Di conseguenza, quando si utilizza MSBuild. exe per compilare e distribuire la soluzione Contact Manager, è necessario:

- Eseguire MSBuild. exe nel file *Publish. proj* .
- Specificare il percorso del file di progetto specifico dell'ambiente fornendo un parametro della riga di comando denominato **TargetEnvPropsFile**.

A tale scopo, il comando MSBuild dovrebbe essere simile al seguente:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]

Da qui, si tratta di un passaggio semplice per passare a una distribuzione ripetibile a singolo passaggio. È sufficiente aggiungere il comando MSBuild a un file. cmd. Nella soluzione Contact Manager la cartella publish include un file denominato *Publish-dev. cmd* che esegue esattamente questa operazione.

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]

> [!NOTE]
> L'opzione **/FL** indica a MSBuild di creare un file di log denominato *MSBuild. log* nella directory di lavoro in cui è stato richiamato MSBuild. exe.

Per distribuire o ridistribuire la soluzione Contact Manager, è sufficiente eseguire il file *Publish-dev. cmd* . Quando si esegue il file, MSBuild eseguirà le operazioni seguenti:

- Compilare tutti i progetti nella soluzione.
- Genera pacchetti web distribuibile per i progetti di applicazione Web.
- Generare file con estensione dbschema e DeployManifest per i progetti di database.
- Distribuire i pacchetti Web sul server Web.
- Distribuire il database nel server di database.

## <a name="run-the-deployment"></a>Eseguire la distribuzione

Quando è stato creato un file di comando per l'ambiente di destinazione, è necessario essere in grado di completare l'intera distribuzione eseguendo semplicemente il file.

**Per distribuire la soluzione Contact Manager nell'ambiente di testing**

1. Nella workstation per sviluppatori aprire Esplora risorse e quindi passare al percorso del file *Publish-dev. cmd* .
2. Fare doppio clic sul file per eseguirlo.
3. Se viene visualizzata la finestra di dialogo **Apri file – avviso di sicurezza** , fare clic su **Esegui**.
4. Se le impostazioni di configurazione e i server di test sono configurati correttamente, nella finestra del prompt dei comandi verrà visualizzato un messaggio di **compilazione completata** Quando MSBuild avrà terminato l'elaborazione dei file di progetto.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Se è la prima volta che la soluzione è stata distribuita in questo ambiente, sarà necessario aggiungere l'account computer del server Web di prova ai ruoli **db\_DataWriter** e **DB\_DataReader** nel database **ContactManager** . Questa procedura è descritta in [configurare un server di database per la pubblicazione distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > È necessario assegnare queste autorizzazioni solo quando si crea il database. Per impostazione predefinita, il processo di compilazione non ricreerà il database in&#x2014;ogni distribuzione, confronterà il database esistente con lo schema più recente e apporterà solo le modifiche necessarie. Di conseguenza, è necessario eseguire il mapping di questi ruoli del database la prima volta che si distribuisce la soluzione.
6. Aprire Internet Explorer e passare all'URL dell'applicazione Contact Manager, ad esempio `http://testweb1:85/ContactManager/`.
7. Verificare che l'applicazione funzioni come previsto e che sia possibile aggiungere contatti.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusione

La creazione di un file di comando contenente le istruzioni MSBuild fornisce un modo rapido e semplice per creare e distribuire una soluzione multiprogetto in un ambiente di destinazione specifico. Se è necessario distribuire ripetutamente la soluzione in più ambienti di destinazione, è possibile creare più file di comando. In ogni file di comando, il comando MSBuild compilerà lo stesso file di progetto universale, ma specifica un file di progetto specifico dell'ambiente diverso. Ad esempio, un file di comando da pubblicare in un ambiente di sviluppo o di test potrebbe contenere questo comando MSBuild:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]

Un file di comando da pubblicare in un ambiente di gestione temporanea potrebbe contenere questo comando MSBuild:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]

> [!NOTE]
> Per istruzioni su come personalizzare i file di progetto specifici dell'ambiente per gli ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

È anche possibile personalizzare il processo di compilazione per ogni ambiente eseguendo l'override delle proprietà o impostando varie altre opzioni nel comando di MSBuild. Per altre informazioni, vedere [riferimenti alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Precedente](deploying-database-projects.md)
> [Successivo](manually-installing-web-packages.md)
