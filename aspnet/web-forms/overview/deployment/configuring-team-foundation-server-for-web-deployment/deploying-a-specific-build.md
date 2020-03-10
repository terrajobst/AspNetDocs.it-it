---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Distribuzione di una compilazione specifica | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come distribuire pacchetti Web e script di database da una compilazione precedente specifica a una nuova destinazione, ad esempio un ambiente di gestione temporanea o di produzione...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6bede6b36c24ade928ab052e14daec1e017bd0b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639671"
---
# <a name="deploying-a-specific-build"></a>Distribuzione di una compilazione specifica

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come distribuire pacchetti Web e script di database da una compilazione precedente specifica a una nuova destinazione, ad esempio un ambiente di gestione temporanea o di produzione.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni è basato sull'approccio del file di progetto suddiviso descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione e distribuzione è&#x2014;controllato da due file di progetto, uno contenente le istruzioni di compilazione applicabili a tutti gli ambienti di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche per l'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica delle attività

Fino ad ora, gli argomenti di questo set di esercitazioni si sono concentrati su come compilare, creare pacchetti e distribuire applicazioni e database Web come parte di un processo singolo o automatizzato. Tuttavia, in alcuni scenari comuni, è opportuno selezionare le risorse distribuite da un elenco di compilazioni in una cartella di ricezione. In altre parole, la build più recente potrebbe non corrispondere alla build che si desidera distribuire.

Si consideri lo scenario di integrazione continua (CI) descritto nell'argomento precedente [creazione di una definizione di compilazione che supporta la distribuzione](creating-a-build-definition-that-supports-deployment.md). In Team Foundation Server (TFS) 2010 è stata creata una definizione di compilazione. Ogni volta che uno sviluppatore controlla il codice in TFS, Team Build compilerà il codice, creerà pacchetti Web e script di database come parte del processo di compilazione, eseguirà qualsiasi unit test e distribuirà le risorse in un ambiente di test. A seconda dei criteri di conservazione configurati durante la creazione della definizione di compilazione, TFS manterrà un certo numero di compilazioni precedenti.

![](deploying-a-specific-build/_static/image1.png)

A questo punto, si supponga di aver eseguito i test di verifica e convalida per una di queste compilazioni nell'ambiente di test e di essere pronti per distribuire l'applicazione in un ambiente di gestione temporanea. Nel frattempo, gli sviluppatori potrebbero aver archiviato nuovo codice. Non si vuole ricompilare la soluzione e distribuirla nell'ambiente di gestione temporanea e non si vuole distribuire la build più recente nell'ambiente di gestione temporanea. Si vuole invece distribuire la compilazione specifica che è stata verificata e convalidata nei server di prova.

A tale scopo, è necessario indicare all'Microsoft Build Engine (MSBuild) dove trovare i pacchetti Web e gli script di database generati da una compilazione specifica.

## <a name="overriding-the-outputroot-property"></a>Override della proprietà OutputRoot

Nella [soluzione di esempio](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), il file *Publish. proj* dichiara una proprietà denominata **OutputRoot**. Come suggerisce il nome, si tratta della cartella radice che contiene tutti gli elementi generati dal processo di compilazione. Nel file *Publish. proj* è possibile notare che la proprietà **OutputRoot** fa riferimento al percorso radice per tutte le risorse di distribuzione.

> [!NOTE]
> **OutputRoot** è un nome di proprietà comunemente utilizzato. Anche C# i file di progetto visivi e Visual Basic dichiarano questa proprietà per archiviare il percorso radice per tutti gli output di compilazione.

[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]

Se si desidera che il file di progetto distribuisca pacchetti Web e script di database da&#x2014;un percorso diverso come gli output di una&#x2014;compilazione TFS precedente, è sufficiente eseguire l'override della proprietà **OutputRoot** . È necessario impostare il valore della proprietà sulla cartella di compilazione pertinente nel server di Team Build. Se MSBuild è stato eseguito dalla riga di comando, è possibile specificare un valore per **OutputRoot** come argomento della riga di comando:

[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]

In pratica, tuttavia, si vuole ignorare anche la destinazione&#x2014;di **compilazione** non è necessario creare la soluzione se non si prevede di usare gli output di compilazione. È possibile eseguire questa operazione specificando le destinazioni che si desidera eseguire dalla riga di comando:

[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]

Tuttavia, nella maggior parte dei casi, è opportuno compilare la logica di distribuzione in una definizione di compilazione TFS. Ciò consente agli utenti con l'autorizzazione per la **compilazione della coda** di attivare la distribuzione da qualsiasi installazione di Visual Studio con una connessione al server TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Creazione di una definizione di compilazione per la distribuzione di compilazioni specifiche

Nella procedura seguente viene descritto come creare una definizione di compilazione che consente agli utenti di attivare le distribuzioni in un ambiente di gestione temporanea con un unico comando.

In questo caso, non si vuole che la definizione di compilazione crei effettivamente&#x2014;tutto ciò che si vuole che esegua la logica di distribuzione nel file di progetto personalizzato. Il file *Publish. proj* include la logica condizionale che ignora la destinazione di **compilazione** se il file è in esecuzione in Team Build. Questa operazione viene eseguita valutando la proprietà **BuildingInTeamBuild** incorporata, che viene impostata automaticamente su **true** se si esegue il file di progetto in Team Build. Di conseguenza, è possibile ignorare il processo di compilazione ed eseguire semplicemente il file di progetto per distribuire una compilazione esistente.

**Per creare una definizione di compilazione per attivare manualmente la distribuzione**

1. In Visual Studio 2010, nella finestra **Team Explorer** espandere il nodo del progetto team, fare clic con il pulsante destro del mouse su **compilazioni**, quindi scegliere **nuova definizione di compilazione**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Nella scheda **generale** assegnare un nome alla definizione di compilazione (ad esempio, **DeployToStaging**) e una descrizione facoltativa.
3. Nella scheda **trigger** selezionare **manuale-archiviazioni non attivano una nuova compilazione**.
4. Nella scheda **impostazioni predefinite compilazione** , nella casella **Copia output di compilazione nella cartella di ricezione seguente** digitare il percorso di Universal Naming Convention (UNC) della cartella di ricezione, ad esempio **\\TFSBUILD\Drops**.

    ![](deploying-a-specific-build/_static/image3.png)
5. Nell'elenco a discesa **file processo di compilazione** della scheda **processo** lasciare selezionata l'opzione **DefaultTemplate. XAML** . Si tratta di uno dei modelli di processo di compilazione predefiniti che vengono aggiunti a tutti i nuovi progetti team.
6. Nella tabella **Parametri processo di compilazione** fare clic nella riga **elementi da compilare** , quindi fare clic sul pulsante con i **puntini** di sospensione.

    ![](deploying-a-specific-build/_static/image4.png)
7. Nella finestra **di dialogo elementi da compilare** fare clic su **Aggiungi**.
8. Nell'elenco a discesa **elementi di tipo** selezionare **file di progetto MSBuild**.
9. Individuare il percorso del file di progetto personalizzato con cui si controlla il processo di distribuzione, selezionare il file e quindi fare clic su **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. Nella finestra **di dialogo elementi da compilare** fare clic su **OK**.
11. Nella tabella **Parametri processo di compilazione** espandere la sezione **Avanzate** .
12. Nella riga **Argomenti MSBuild** specificare il percorso del file di progetto specifico dell'ambiente e aggiungere un segnaposto per il percorso della cartella di compilazione:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > È necessario eseguire l'override del valore di **OutputRoot** ogni volta che si accoda una compilazione. Questo argomento viene trattato nella procedura seguente.
13. Fare clic su **Salva**.

Quando si attiva una compilazione, è necessario aggiornare la proprietà **OutputRoot** in modo che punti alla compilazione che si desidera distribuire.

**Per distribuire una compilazione specifica da una definizione di compilazione**

1. Nella finestra di **Team Explorer** , fare clic con il pulsante destro del mouse sulla definizione di compilazione, quindi scegliere **Accoda nuova compilazione**.

    ![](deploying-a-specific-build/_static/image7.png)
2. Nella scheda **parametri** della finestra di dialogo **compilazione coda** espandere la sezione **Avanzate** .
3. Nella riga degli **argomenti di MSBuild** sostituire il valore della proprietà **OutputRoot** con il percorso della cartella di compilazione. Esempio:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Assicurarsi di includere una barra finale alla fine del percorso della cartella di compilazione.
4. Fare clic su **coda**.

Quando si accoda la compilazione, il file di progetto distribuirà gli script di database e i pacchetti Web dalla cartella di destinazione della compilazione specificata nella proprietà **OutputRoot** .

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come pubblicare le risorse di distribuzione, ad esempio i pacchetti Web e gli script di database, da una compilazione precedente specifica utilizzando il modello di distribuzione del file Split Project. È stato illustrato come eseguire l'override della proprietà **OutputRoot** e come incorporare la logica di distribuzione in una definizione di compilazione TFS.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla creazione di definizioni di compilazione, vedere [creare una definizione di compilazione di base](https://msdn.microsoft.com/library/ms181716.aspx) e [definire il processo di compilazione](https://msdn.microsoft.com/library/ms181715.aspx). Per ulteriori informazioni sulle compilazioni di Accodamento, vedere [accodare una compilazione](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Precedente](creating-a-build-definition-that-supports-deployment.md)
> [Successivo](configuring-permissions-for-team-build-deployment.md)
