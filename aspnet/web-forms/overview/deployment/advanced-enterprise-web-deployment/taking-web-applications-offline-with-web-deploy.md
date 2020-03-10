---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Esecuzione offline di applicazioni Web con Distribuzione Web | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come portare offline un'applicazione Web per la durata di una distribuzione automatizzata tramite il avvis Web di Internet Information Services (IIS)...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: ba60664a0c3daa0650cd7e7cfc4ab9da08df3440
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526068"
---
# <a name="taking-web-applications-offline-with-web-deploy"></a>Impostazione delle applicazioni Web in modalità offline con Distribuzione Web

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come portare offline un'applicazione Web per la durata di una distribuzione automatizzata mediante lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web). Gli utenti che selezionano l'applicazione Web vengono reindirizzati a un' *App\_file offline. htm* fino al completamento della distribuzione.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica delle attività

In numerosi scenari, è consigliabile portare offline un'applicazione Web mentre si apportano modifiche ai componenti correlati, ad esempio i database o i servizi Web. In genere, in IIS e ASP.NET, è possibile eseguire questa operazione inserendo un file denominato *App\_offline. htm* nella cartella radice del sito Web IIS o dell'applicazione Web. L' *App\_file offline. htm* è un file HTML standard e in genere contiene un semplice messaggio che avvisa l'utente che il sito è temporaneamente non disponibile a causa della manutenzione. Mentre l' *App\_file offline. htm* è presente nella cartella radice del sito Web, IIS reindirizza automaticamente le richieste al file. Al termine dell'esecuzione degli aggiornamenti, rimuovere l' *App\_file offline. htm* e il sito Web riprende a servire le richieste come di consueto.

Quando si usa Distribuzione Web per eseguire distribuzioni automatiche o a singolo passaggio in un ambiente di destinazione, è possibile incorporare l'aggiunta e la rimozione dell' *App\_file offline. htm* nel processo di distribuzione. A tale scopo, è necessario completare le attività di alto livello:

- Nel file di progetto Microsoft Build Engine (MSBuild) usato per controllare il processo di distribuzione, creare una destinazione di MSBuild che copia un' *App\_file offline. htm* nel server di destinazione prima di iniziare qualsiasi attività di distribuzione.
- Aggiungere un'altra destinazione MSBuild che rimuove l' *App\_file offline. htm* dal server di destinazione al termine di tutte le attività di distribuzione.
- Nel progetto di applicazione Web creare un file con *estensione WPP. targets* che assicuri che un' *app\_file offline. htm* venga aggiunto al pacchetto di distribuzione quando viene richiamato distribuzione Web.

In questo argomento viene illustrato come eseguire queste procedure. Le attività e le procedure dettagliate riportate in questo argomento presuppongono che sia già stata creata una soluzione che contiene almeno un progetto di applicazione Web e di usare un file di progetto personalizzato per controllare il processo di distribuzione come descritto in [distribuzione Web nell'azienda](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). In alternativa, è possibile usare la soluzione di esempio [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) per seguire gli esempi nell'argomento.

## <a name="adding-an-app_offline-file-to-a-web-application-project"></a>Aggiunta di un'app\_file offline a un progetto di applicazione Web

Per completare la prima attività è necessario aggiungere un' *App\_file offline* al progetto di applicazione Web:

- Per evitare che il file interferisca con il processo di sviluppo (non si vuole che l'applicazione sia in modalità non in linea), è consigliabile chiamarla a parte l' *App\_offline. htm*. Ad esempio, è possibile assegnare al file l' *App\_offline-template. htm*.
- Per impedire che il file venga distribuito così com'è, è necessario impostare l'azione di compilazione su **nessuno**.

**Per aggiungere un'app\_file offline a un progetto di applicazione Web**

1. Aprire la soluzione in Visual Studio 2010.
2. Nella finestra di **Esplora soluzioni** , fare clic con il pulsante destro del mouse sul progetto di applicazione Web, scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.
3. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **pagina HTML**.
4. Nella casella **nome** digitare **app\_offline-template. htm**, quindi fare clic su **Aggiungi**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Aggiungere un codice HTML semplice per informare gli utenti che l'applicazione non è disponibile e quindi salvare il file. Non includere tag sul lato server, ad esempio i tag con prefisso "ASP:". 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. Nella finestra di **Esplora soluzioni** , fare clic con il pulsante destro del mouse sul nuovo file, quindi scegliere **Proprietà**.
7. Nella riga **azione di compilazione** della finestra **Proprietà** selezionare **nessuno**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-app_offline-file"></a>Distribuzione ed eliminazione di un'app\_file offline

Il passaggio successivo consiste nel modificare la logica di distribuzione per copiare il file nel server di destinazione all'inizio del processo di distribuzione e rimuoverlo alla fine.

> [!NOTE]
> Nella procedura seguente si presuppone che si stia usando un file di progetto MSBuild personalizzato per controllare il processo di distribuzione, come descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Se si esegue la distribuzione diretta da Visual Studio, è necessario usare un approccio diverso. Sayed Ibrahim Hashimi descrive un approccio di questo tipo in [come portare offline l'app Web durante la pubblicazione](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).

Per distribuire un' *App\_file offline* in un sito Web IIS di destinazione, è necessario richiamare MSDeploy. exe usando il [provider di distribuzione Web **contentPath** ](https://technet.microsoft.com/library/dd569034(WS.10).aspx). Il provider **contentPath** supporta sia i percorsi di directory fisici sia i percorsi di applicazioni o siti Web IIS, che lo rendono la scelta ideale per la sincronizzazione di un file tra una cartella di progetto di Visual Studio e un'applicazione Web IIS. Per distribuire il file, il comando MSDeploy dovrebbe essere simile al seguente:

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]

Per rimuovere il file dal sito di destinazione al termine del processo di distribuzione, il comando MSDeploy dovrebbe essere simile al seguente:

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]

Per automatizzare questi comandi come parte di un processo di compilazione e distribuzione, è necessario integrarli nel file di progetto MSBuild personalizzato. Nella procedura seguente viene descritto come eseguire questa operazione.

**Per distribuire ed eliminare un'app\_file offline**

1. In Visual Studio 2010 aprire il file di progetto MSBuild che controlla il processo di distribuzione. Nella soluzione di esempio [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , si tratta del file *Publish. proj* .
2. Nell'elemento del **progetto** radice creare un nuovo elemento **PropertyGroup** per archiviare le variabili per l' *app\_distribuzione offline* :

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. La proprietà **SourceRoot** è definita in un'altra posizione nel file *Publish. proj* . Indica il percorso della cartella radice per il contenuto di origine relativo al percorso&#x2014;corrente in altre parole, relativo al percorso del file *Publish. proj* .
4. Il provider **contentPath** non accetta i percorsi di file relativi, quindi è necessario ottenere un percorso assoluto del file di origine prima di poterlo distribuire. A tale scopo, è possibile usare l'attività [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) .
5. Aggiungere un nuovo elemento di **destinazione** denominato **GetAppOfflineAbsolutePath**. All'interno di questa destinazione, usare l'attività **ConvertToAbsolutePath** per ottenere un percorso assoluto dell' *app\_file del modello offline* nella cartella del progetto.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Questa destinazione accetta il percorso relativo dell' *App\_file del modello offline* nella cartella del progetto e lo salva in una nuova proprietà come percorso assoluto del file. L'attributo **BeforeTargets** specifica che si vuole che questa destinazione venga eseguita prima della destinazione **DeployAppOffline** , che verrà creata nel passaggio successivo.
7. Aggiungere una nuova destinazione denominata **DeployAppOffline**. All'interno di questa destinazione, richiamare il comando MSDeploy. exe che distribuisce l' *App\_file offline* nel server Web di destinazione.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. In questo esempio, la proprietà **ContactManagerIisPath** è definita in un'altra posizione nel file di progetto. Si tratta semplicemente di un percorso di applicazione IIS, nel formato *[nome sito Web IIS]/[nome applicazione]* . L'inclusione di una condizione nella destinazione consente agli utenti di attivare o disattivare l' *App\_distribuzione offline* modificando il valore di una proprietà o specificando un parametro della riga di comando.
9. Aggiungere una nuova destinazione denominata **DeleteAppOffline**. All'interno di questa destinazione, richiamare il comando MSDeploy. exe che rimuove l' *App\_file offline* dal server Web di destinazione.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. L'attività finale consiste nel richiamare queste nuove destinazioni nei punti appropriati durante l'esecuzione del file di progetto. Questa operazione può essere eseguita in vari modi. Nel file *Publish. proj* , ad esempio, la proprietà **FullPublishDependsOn** specifica un elenco di destinazioni che devono essere eseguite in ordine quando viene richiamata la destinazione predefinita **FullPublish** .
11. Modificare il file di progetto MSBuild per richiamare le destinazioni **DeployAppOffline** e **DeleteAppOffline** nei punti appropriati del processo di pubblicazione.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Quando si esegue il file di progetto MSBuild personalizzato, l' *App\_file offline* verrà distribuita nel server immediatamente dopo una compilazione completata. Verrà quindi eliminato dal server una volta completate tutte le attività di distribuzione.

## <a name="adding-an-app_offline-file-to-deployment-packages"></a>Aggiunta di un'app\_file offline ai pacchetti di distribuzione

A seconda della modalità di configurazione della distribuzione, qualsiasi contenuto esistente nell'applicazione&#x2014;Web IIS di destinazione come l' *app\_file* &#x2014;offline. htm può essere eliminato automaticamente quando si distribuisce un pacchetto Web nella destinazione. Per assicurarsi che l' *App\_file offline. htm* rimanga sul posto per la durata della distribuzione, è necessario includere il file nel pacchetto di distribuzione Web stesso, oltre a distribuire il file direttamente all'inizio del processo di distribuzione.

- Se sono state seguite le attività precedenti di questo argomento, è stata aggiunta l' *app\_file offline. htm* al progetto di applicazione Web con un nome file diverso (è stata usata l' *app\_offline-template. htm*) e l'azione di compilazione è stata impostata su **None**. Queste modifiche sono necessarie per impedire che il file interferisca con lo sviluppo e il debug. Di conseguenza, è necessario personalizzare il processo di creazione dei pacchetti per assicurarsi che l' *App\_file offline. htm* sia incluso nel pacchetto di distribuzione Web.

La pipeline di pubblicazione sul Web (WPP) utilizza un elenco di elementi denominato **FilesForPackagingFromProject** per compilare un elenco di file che devono essere inclusi nel pacchetto di distribuzione Web. È possibile personalizzare il contenuto dei pacchetti Web aggiungendo elementi personalizzati a questo elenco. A tale scopo, è necessario completare questi passaggi di alto livello:

1. Creare un file di progetto personalizzato denominato *[nome progetto]. WPP. targets* nella stessa cartella del file di progetto.

    > [!NOTE]
    > Il file *. WPP. targets* deve essere inserito nella stessa cartella del file&#x2014;di progetto dell'applicazione Web, ad esempio *ContactManager. Mvc. csproj*&#x2014;anziché nella stessa cartella dei file di progetto personalizzati usati per controllare il processo di compilazione e distribuzione.
2. Nel file con estensione *WPP. targets* creare una nuova destinazione di MSBuild che viene eseguita *prima* della destinazione **CopyAllFilesToSingleFolderForPackage** . Si tratta della destinazione WPP che compila un elenco di elementi da includere nel pacchetto.
3. Nella nuova destinazione creare un elemento **ItemGroup** .
4. Nell'elemento **ItemGroup** aggiungere un elemento **FilesForPackagingFromProject** e specificare l' *app\_file offline. htm* .

Il file con estensione *WPP. targets* dovrebbe essere simile al seguente:

[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]

Si tratta dei punti chiave di nota in questo esempio:

- L'attributo **BeforeTargets** inserisce questa destinazione in WPP specificando che deve essere eseguita immediatamente prima della destinazione **CopyAllFilesToSingleFolderForPackage** .
- L'elemento **FilesForPackagingFromProject** usa il valore dei metadati **DestinationRelativePath** per rinominare il file dall' *app\_offline-template. htm* all' *app\_offline. htm* perché viene aggiunto all'elenco.

Nella procedura seguente viene illustrato come aggiungere il file con *estensione WPP. targets* a un progetto di applicazione Web.

**Per aggiungere un file con estensione WPP. targets a un pacchetto di distribuzione Web**

1. Aprire la soluzione in Visual Studio 2010.
2. Nella finestra di **Esplora soluzioni** , fare clic con il pulsante destro del mouse sul nodo del progetto di applicazione Web (ad esempio, **ContactManager. Mvc**), scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.
3. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello di **file XML** .
4. Nella casella **nome** Digitare *[nome progetto] * * *. WPP. targets** (ad esempio, **ContactManager. Mvc. WPP. targets**), quindi fare clic su **Aggiungi**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Se si aggiunge un nuovo elemento al nodo radice di un progetto, il file viene creato nella stessa cartella del file di progetto. È possibile verificarlo aprendo la cartella in Esplora risorse.
5. Nel file aggiungere il markup di MSBuild descritto in precedenza.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Salvare e chiudere il file *[nome progetto]. WPP. targets* .

La volta successiva che si compila e si crea il pacchetto del progetto di applicazione Web, WPP rileverà automaticamente il file con *estensione WPP. targets* . Il file *app\_offline-template. htm* verrà incluso nel pacchetto di distribuzione Web risultante come *app\_offline. htm*.

> [!NOTE]
> Se la distribuzione non riesce, l' *App\_file offline. htm* rimarrà sul posto e l'applicazione rimarrà offline. Si tratta in genere del comportamento desiderato. Per riportare online l'applicazione, è possibile eliminare l' *App\_file offline. htm* dal server Web. In alternativa, se si correggono gli errori ed è stata eseguita una distribuzione corretta, l' *App\_file offline. htm* verrà rimosso.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come portare offline un'applicazione Web per la durata di una distribuzione, pubblicando un' *App\_file offline. htm* nel server di destinazione all'inizio del processo di distribuzione e rimuoverlo alla fine. È stato inoltre illustrato come includere un' *App\_file offline. htm* in un pacchetto di distribuzione Web.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sul processo di creazione di pacchetti e distribuzione, vedere [compilazione e creazione](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)di pacchetti di progetti di applicazioni Web, [configurazione di parametri per la distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)e [distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Se si pubblicano le applicazioni Web direttamente da Visual Studio, invece di usare l'approccio personalizzato per i file di progetto MSBuild descritto in queste esercitazioni, sarà necessario usare un approccio leggermente diverso per portare offline l'applicazione durante la pubblicazione processo.

> [!div class="step-by-step"]
> [Precedente](excluding-files-and-folders-from-deployment.md)
> [Successivo](running-windows-powershell-scripts-from-msbuild-project-files.md)
