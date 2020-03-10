---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Installazione manuale dei pacchetti Web | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come importare manualmente un pacchetto di distribuzione Web in Internet Information Services (IIS). L'argomento compilazione e creazione di pacchetti Web applicazione...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: f778549d3e26989a2e71ef21171adec521842729
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634204"
---
# <a name="manually-installing-web-packages"></a>Installazione manuale dei pacchetti Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come importare manualmente un pacchetto di distribuzione Web in Internet Information Services (IIS).
> 
> In questo argomento viene descritto come lo strumento di distribuzione Web IIS (Distribuzione Web), insieme al Microsoft Build Engine (MSBuild) e alla pipeline di pubblicazione Web (WPP), consente di [creare](building-and-packaging-web-application-projects.md) un pacchetto dei progetti di applicazione Web in un unico file zip. Questo file, comunemente noto come pacchetto di distribuzione Web (o semplicemente un pacchetto di distribuzione), contiene tutti i contenuti e le informazioni di configurazione necessarie per IIS per ricreare l'applicazione Web in un server Web.
> 
> Una volta creato un pacchetto di distribuzione Web, è possibile pubblicarlo in un server IIS in diversi modi. In numerosi scenari, è opportuno sfruttare i punti di integrazione tra MSBuild, WPP e Distribuzione Web per creare e installare i pacchetti Web in modalità remota come parte di un processo di compilazione e distribuzione automatizzato o in un singolo passaggio. Questo processo è descritto in [distribuzione di pacchetti Web](deploying-web-packages.md). Tuttavia, questo non è sempre possibile. Si supponga di voler distribuire un'applicazione Web in un ambiente di produzione con connessione Internet. Per motivi di sicurezza, un ambiente di produzione di questo tipo è molto meno probabile che si trovi dietro un firewall in una subnet separata dal server di compilazione, in una rete perimetrale (nota anche come subnet schermata). In molti casi, l'ambiente di produzione si troverà in un dominio separato o in una rete fisicamente isolata.
> 
> In questi scenari, l'unica opzione possibile consiste nel trasferire il pacchetto Web nel server di destinazione e importarlo manualmente in IIS. Sebbene questo approccio escluda la distribuzione automatizzata, è ancora una tecnica estremamente efficace per la pubblicazione di&#x2014;un'applicazione Web è sufficiente copiare un singolo file zip nel server Web e utilizzare una procedura guidata per guidare l'utente nel processo di importazione.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

## <a name="task-overview"></a>Panoramica delle attività

È necessario completare queste attività di alto livello per importare un pacchetto di distribuzione Web in IIS:

- Creare un pacchetto di distribuzione Web usando la riga di comando di MSBuild, Team Build o Visual Studio 2010.
- Copiare il pacchetto Web nel server Web di destinazione.
- Utilizzare la procedura guidata Importa pacchetto di applicazioni in Gestione IIS per installare il pacchetto Web e fornire valori per variabili quali le stringhe di connessione e gli endpoint di servizio.

In questo argomento viene illustrato come eseguire queste procedure. Le attività e le procedure dettagliate riportate in questo argomento presuppongono che si abbia già familiarità con i concetti alla base dei pacchetti Web, Distribuzione Web e WPP. Per altre informazioni, vedere [compilazione e creazione di pacchetti di progetti di applicazione Web](building-and-packaging-web-application-projects.md).

> [!NOTE]
> Questo argomento viene usato in combinazione con [la configurazione di un server Web per la pubblicazione distribuzione Web (distribuzione offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), che spiega come installare i componenti necessari e preparare un sito Web IIS per l'importazione del pacchetto.

## <a name="create-a-web-deployment-package"></a>Creazione di un pacchetto di distribuzione Web

La prima attività consiste nel creare un pacchetto di distribuzione Web per il progetto di applicazione Web che si desidera distribuire. È possibile creare pacchetti Web in diversi modi.

**Approccio 1: creare un pacchetto come parte del processo di compilazione con Visual Studio**

È possibile configurare il progetto di applicazione Web per creare un pacchetto di distribuzione Web dopo ogni compilazione tramite la scheda **Pubblicazione/creazione pacchetto Web** nelle pagine delle proprietà del progetto. Questo processo è descritto in [compilazione e creazione di pacchetti di progetti di applicazione Web](building-and-packaging-web-application-projects.md).

**Approccio 2: creare un pacchetto come parte del processo di compilazione con MSBuild**

Se si compila il progetto di applicazione Web tramite MSBuild direttamente, tramite un file di progetto MSBuild personalizzato o dalla riga di comando, è possibile creare un pacchetto di distribuzione Web come parte del processo di compilazione includendo le proprietà **DeployOnBuild = true** e **DeployTarget = Package** nel comando. Questo processo è descritto in [informazioni sul processo di compilazione](understanding-the-build-process.md).

**Approccio 3: creare un pacchetto su richiesta in Visual Studio**

È possibile creare un pacchetto di distribuzione Web per un progetto di applicazione Web in qualsiasi momento in Visual Studio 2010. A tale scopo, nella finestra **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto di applicazione Web, quindi scegliere **Compila pacchetto di distribuzione**.

![](manually-installing-web-packages/_static/image1.png)

**Approccio 4: creare un pacchetto su richiesta dalla riga di comando**

È possibile creare un pacchetto di distribuzione Web dalla riga di comando richiamando la destinazione del **pacchetto** nel progetto di applicazione Web tramite MSBuild. Il comando dovrebbe essere simile al seguente:

[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]

Indipendentemente dall'approccio usato, il risultato finale è lo stesso. WPP crea un pacchetto di distribuzione Web come file zip, insieme a varie risorse di supporto, nella cartella di output per il progetto di applicazione Web.

![](manually-installing-web-packages/_static/image2.png)

Quando si prevede di importare il pacchetto Web manualmente, è necessario solo il file zip. Copiare questo file nel server Web di destinazione ed è possibile avviare il processo di importazione.

## <a name="import-a-web-package-into-iis"></a>Importare un pacchetto Web in IIS

È possibile utilizzare la procedura seguente per importare un pacchetto di distribuzione Web dal file system locale in un sito Web IIS. Prima di eseguire questa procedura, assicurarsi di disporre di:

- Copia del pacchetto di distribuzione Web nel server Web.
- Configurazione di un server Web IIS per ospitare l'applicazione.

Per ulteriori informazioni sulla configurazione di un server Web IIS per supportare i pacchetti di distribuzione Web, vedere [configurare un server Web per la pubblicazione distribuzione Web (distribuzione offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Per importare un pacchetto di distribuzione Web tramite Gestione IIS**

1. Nel riquadro **connessioni** di gestione IIS fare clic con il pulsante destro del mouse sul sito Web IIS, scegliere **Distribuisci**e quindi fare clic su **Importa applicazione**.

    ![](manually-installing-web-packages/_static/image3.png)
2. Nella procedura guidata Importa pacchetto di applicazioni, nella pagina **selezionare il pacchetto** , passare al percorso del pacchetto di distribuzione Web e quindi fare clic su **Avanti**.
3. Nella pagina **selezionare il contenuto del pacchetto deselezionare il** contenuto non necessario, quindi fare clic su **Avanti**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > In numerosi casi, è possibile che non si desideri importare tutti gli elementi forniti con un pacchetto di distribuzione Web. Ad esempio, potrebbe non essere necessario consentire Distribuzione Web di sostituire il database associato.  
    > Le voci **Grant Permissions** impostano le autorizzazioni per la destinazione file System per garantire che l'identità del pool di applicazioni possa accedere alla cartella fisica in cui è archiviato il contenuto del sito Web. All'utente di autenticazione anonima viene inoltre concessa l'autorizzazione di lettura per la cartella per consentire all'applicazione di gestire i file di tipo MIME (Multipurpose Internet Mail Extensions). Se si preferisce, è possibile rimuovere queste voci e configurare le autorizzazioni manualmente.
4. Nella pagina **immettere le informazioni sul pacchetto dell'applicazione** specificare le informazioni richieste.

    ![](manually-installing-web-packages/_static/image5.png)
5. Quando si crea un pacchetto Web, WPP analizza il file di configurazione per l'applicazione e rileva le variabili, ad esempio le stringhe di connessione e gli endpoint di servizio. In questo caso:

    1. **Percorso applicazione** è il percorso di IIS in cui si desidera installare l'applicazione. Questa impostazione è comune a tutti i pacchetti di distribuzione creati dal WPP.
    2. L' **indirizzo dell'endpoint del servizio ContactService** è l'indirizzo che l'applicazione deve usare per comunicare con il servizio WCF distribuito. Questa impostazione corrisponde a una voce nel file *Web. config* .
    3. La prima impostazione della **stringa di connessione** è la stringa di connessione che distribuzione Web deve usare per distribuire il database associato all'applicazione, in questo caso un database di appartenenza a ASP.NET. Questa impostazione corrisponde all'impostazione nella scheda **Pubblicazione/creazione pacchetto SQL** in Visual Studio.
    4. La seconda impostazione della **stringa di connessione** è la stringa di connessione che verrà effettivamente usata dall'applicazione per comunicare con il database quando è in esecuzione. Corrisponde a una voce della stringa di connessione nel file *Web. config* .

        > [!NOTE]
        > Per ulteriori informazioni sulla provenienza di questi parametri, vedere [configurazione dei parametri per la distribuzione del pacchetto Web](configuring-parameters-for-web-package-deployment.md).
6. Fare clic su **Avanti**.
7. Se questa non è la prima volta che si distribuisce l'applicazione in questo sito Web, verrà richiesto di specificare se si desidera eliminare tutto il contenuto esistente prima dell'installazione. Scegliere l'opzione appropriata per i propri requisiti e quindi fare clic su **Avanti**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Al termine dell'installazione del pacchetto da parte di IIS, fare clic su **fine**.

    ![](manually-installing-web-packages/_static/image7.png)

A questo punto, la pubblicazione dell'applicazione Web in IIS è stata completata.

## <a name="conclusion"></a>Conclusione

Questo argomento descrive come importare un pacchetto di distribuzione Web in un sito Web IIS usando Gestione IIS. Questo approccio alla pubblicazione di applicazioni Web è appropriato quando i vincoli di sicurezza o di infrastruttura rendono impossibile o indesiderabile la distribuzione remota.

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni su come configurare un server Web IIS per supportare l'importazione manuale di un pacchetto Web, vedere [configurare un server Web per la pubblicazione distribuzione Web (distribuzione offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Per indicazioni generali sulla distribuzione di pacchetti Web, vedere [procedura dettagliata: distribuzione di un progetto di applicazione Web tramite un pacchetto di distribuzione Web (parte 1 di 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Precedente](creating-and-running-a-deployment-command-file.md)
