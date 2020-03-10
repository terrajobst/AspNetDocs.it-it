---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Creazione di una definizione di compilazione che supporta la distribuzione | Microsoft Docs
author: jrjlee
description: Se si desidera eseguire qualsiasi tipo di compilazione in Team Foundation Server (TFS) 2010, è necessario creare una definizione di compilazione all'interno del progetto team. Questo argomento des...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: e11c91a824446572aaf0b3bc6954b9b8ffb4eaff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603999"
---
# <a name="creating-a-build-definition-that-supports-deployment"></a>Creazione di una definizione di compilazione che supporta la distribuzione

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Se si desidera eseguire qualsiasi tipo di compilazione in Team Foundation Server (TFS) 2010, è necessario creare una definizione di compilazione all'interno del progetto team. In questo argomento viene descritto come creare una nuova definizione di compilazione in TFS e come controllare la distribuzione Web come parte del processo di compilazione in Team Build.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni è basato sull'approccio del file di progetto suddiviso descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione e distribuzione è&#x2014;controllato da due file di progetto, uno contenente le istruzioni di compilazione applicabili a tutti gli ambienti di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche per l'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica delle attività

Una definizione di compilazione è il meccanismo che controlla come e quando si verificano le compilazioni per i progetti team in TFS. Ogni definizione di compilazione specifica:

- Gli elementi che si desidera compilare, ad esempio i file di soluzione di Visual Studio o i file di progetto di Microsoft Build Engine personalizzati (MSBuild).
- Criteri che determinano quando deve essere eseguita una compilazione, ad esempio trigger manuali, integrazione continua (CI) o archiviazioni gestite.
- Percorso a cui Team Build deve inviare gli output di compilazione, inclusi elementi di distribuzione come pacchetti Web e script di database.
- Quantità di tempo in cui ogni compilazione deve essere mantenuta.
- Diversi altri parametri del processo di compilazione.

> [!NOTE]
> Per altre informazioni sulle definizioni di compilazione, vedere [definire il processo di compilazione](https://msdn.microsoft.com/library/ms181715.aspx).

In questo argomento viene illustrato come creare una definizione di compilazione che usa CI, in modo che venga attivata una compilazione quando uno sviluppatore archivia nuovi contenuti. Se la compilazione ha esito positivo, il servizio di compilazione esegue un file di progetto personalizzato per distribuire la soluzione in un ambiente di test.

Quando si attiva una compilazione, è necessario eseguire queste azioni:

- In primo luogo, Team Build deve compilare la soluzione. Nell'ambito di questo processo, Team Build richiama la pipeline di pubblicazione Web (WPP) per generare pacchetti di distribuzione Web per ogni progetto di applicazione Web nella soluzione. Team Build eseguirà anche tutti gli unit test associati alla soluzione.
- Se la compilazione della soluzione ha esito negativo, Team Build non deve eseguire altre azioni. Gli errori degli unit test devono essere considerati come un errore di compilazione.
- Se la compilazione della soluzione riesce, Team Build deve eseguire il file di progetto personalizzato che controlla la distribuzione della soluzione. Nell'ambito di questo processo, Team Build richiamerà lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web) per installare le applicazioni Web in pacchetto nei server Web di destinazione e richiamerà l'utilità VSDBCMD. exe per eseguire la creazione del database. script nei server di database di destinazione.

Viene illustrato il processo:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

La soluzione di esempio [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) include un file di progetto MSBuild personalizzato, *Publish. proj*, che è possibile eseguire da MSBuild o Team Build. Come descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), questo file di progetto definisce la logica che consente di distribuire i pacchetti e i database Web in un ambiente di destinazione. Il file include la logica che omette il processo di compilazione e creazione di pacchetti se è in esecuzione in Team Build, lasciando solo le attività di distribuzione da eseguire. Questo perché quando si automatizza la distribuzione in questo modo, in genere si vuole assicurarsi che la soluzione venga compilata correttamente e che passi tutti gli unit test prima che il processo di distribuzione inizi.

Nella sezione successiva viene illustrato come implementare questo processo creando una nuova definizione di compilazione.

> [!NOTE]
> Questa procedura&#x2014;in cui un singolo processo automatizzato compila, testa e distribuisce una soluzione&#x2014;è probabilmente più adatta per la distribuzione negli ambienti di test. Per gli ambienti di gestione temporanea e di produzione è molto più probabile che si desideri distribuire contenuto da una compilazione precedente già verificata e convalidata in un ambiente di testing. Questo approccio è descritto nell'argomento successivo relativo alla [distribuzione di una compilazione specifica](deploying-a-specific-build.md).

### <a name="who-performs-this-procedure"></a>Chi esegue questa procedura?

In genere, un amministratore TFS esegue questa procedura. In alcuni casi, un responsabile del team di sviluppo può assumersi la responsabilità della raccolta di progetti team in TFS. Per creare una nuova definizione di compilazione, è necessario essere un membro del gruppo **Project Collection Build Administrators** per la raccolta di progetti team che contiene la soluzione.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Creare una definizione di compilazione per CI e la distribuzione

Nella procedura seguente viene descritto come creare una definizione di compilazione attivata da CI. Se la compilazione ha esito positivo, la soluzione viene distribuita usando la logica in un file di progetto MSBuild personalizzato.

**Per creare una definizione di compilazione per CI e la distribuzione**

1. In Visual Studio 2010, nella finestra **Team Explorer** espandere il nodo del progetto team, fare clic con il pulsante destro del mouse su **compilazioni**, quindi scegliere **nuova definizione di compilazione**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Nella scheda **generale** assegnare un nome alla definizione di compilazione (ad esempio, **DeployToTest**) e una descrizione facoltativa.
3. Nella scheda **trigger** selezionare i criteri per i quali si desidera attivare una nuova compilazione. Se ad esempio si vuole compilare la soluzione e distribuirla nell'ambiente di test ogni volta che uno sviluppatore archivia un nuovo codice, selezionare **integrazione continua**.
4. Nella scheda **impostazioni predefinite compilazione** , nella casella **Copia output di compilazione nella cartella di ricezione seguente** digitare il percorso di Universal Naming Convention (UNC) della cartella di ricezione, ad esempio **\\TFSBUILD\Drops**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Questo percorso di rilascio archivia diverse compilazioni, a seconda dei criteri di conservazione configurati. Quando si desidera pubblicare elementi di distribuzione da una compilazione specifica in un ambiente di gestione temporanea o di produzione, è possibile trovarli qui.
5. Nell'elenco a discesa **file processo di compilazione** della scheda **processo** lasciare selezionata l'opzione **DefaultTemplate. XAML** . Si tratta di uno dei modelli di processo di compilazione predefiniti che vengono aggiunti a tutti i nuovi progetti team.
6. Nella tabella **Parametri processo di compilazione** fare clic nella riga **elementi da compilare** , quindi fare clic sul pulsante con i **puntini** di sospensione.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. Nella finestra **di dialogo elementi da compilare** fare clic su **Aggiungi**.
8. Individuare il percorso del file della soluzione e quindi fare clic su **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. Nella finestra **di dialogo elementi da compilare** fare clic su **Aggiungi**.
10. Nell'elenco a discesa **elementi di tipo** selezionare **file di progetto MSBuild**.
11. Individuare il percorso del file di progetto personalizzato con cui si controlla il processo di distribuzione, selezionare il file e quindi fare clic su **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. Nella finestra **di dialogo elementi da compilare** verranno ora visualizzati due elementi. Fare clic su **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Nella tabella **Parametri processo di compilazione** della scheda **processo** espandere la sezione **Avanzate** .
14. Nella riga **Argomenti MSBuild** aggiungere eventuali argomenti della riga di comando di MSBuild per i quali è richiesto *uno* degli elementi da compilare. Nello scenario della soluzione Contact Manager, questi argomenti sono obbligatori:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. In questo esempio:

    1. Gli argomenti **DeployOnBuild = true** e **DeployTarget = Package** sono obbligatori quando si compila la soluzione Contact Manager. In questo modo si indica a MSBuild di creare pacchetti di distribuzione Web dopo aver compilato ogni progetto di applicazione Web, come descritto in [compilazione e creazione di pacchetti di progetti di applicazione Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. L'argomento **TargetEnvPropsFile** è obbligatorio quando si compila il file *Publish. proj* . Questa proprietà indica il percorso del file di configurazione specifico dell'ambiente, come descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Nella scheda **criteri di conservazione** configurare il numero di compilazioni di ogni tipo che si desidera mantenere come richiesto.
17. Fare clic su **Salva**.

## <a name="queue-a-build"></a>Accodare una compilazione

A questo punto, è stata creata almeno una nuova definizione di compilazione. Il processo di compilazione definito verrà ora eseguito in base ai trigger specificati nella definizione di compilazione.

Se la definizione di compilazione è stata configurata in modo da usare CI, è possibile testare la definizione di compilazione in due modi:

- Archiviare un contenuto nel progetto team per attivare una compilazione automatica.
- Accodare manualmente una compilazione.

**Per accodare una compilazione manualmente**

1. Nella finestra di **Team Explorer** , fare clic con il pulsante destro del mouse sulla definizione di compilazione, quindi scegliere **Accoda nuova compilazione**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. Nella finestra di dialogo **Accoda compilazione** esaminare le proprietà di compilazione e quindi fare clic su **Accoda**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Per esaminare lo stato di avanzamento e il risultato di&#x2014;una compilazione indipendentemente dal fatto che sia stato attivato manualmente&#x2014;o facendo doppio clic sulla definizione di compilazione nella finestra **Team Explorer** . Verrà aperta una scheda **Esplora compilazione** .

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Da qui è possibile risolvere i problemi relativi alle compilazioni non riuscite. Se si fa doppio clic su una singola compilazione, è possibile visualizzare le informazioni di riepilogo e fare clic su file di log dettagliati.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

È possibile utilizzare queste informazioni per risolvere i problemi relativi alle compilazioni non riuscite e risolvere eventuali problemi prima di tentare un'altra compilazione.

> [!NOTE]
> Le compilazioni che eseguono la logica di distribuzione hanno probabilmente esito negativo fino a quando non viene concesso al server di compilazione le autorizzazioni necessarie nell'ambiente di destinazione. Per ulteriori informazioni, vedere [configurazione delle autorizzazioni per la distribuzione di Team Build](configuring-permissions-for-team-build-deployment.md).

## <a name="monitor-the-build-process"></a>Monitorare il processo di compilazione

TFS offre un'ampia gamma di funzionalità che consentono di monitorare il processo di compilazione. Ad esempio, TFS può inviare un messaggio di posta elettronica o visualizzare avvisi nell'area di notifica della barra delle applicazioni quando una compilazione è stata completata. Per altre informazioni, vedere [Run and monitor Builds](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come creare una definizione di compilazione in TFS. La definizione di compilazione è configurata per CI, quindi il processo di compilazione viene eseguito ogni volta che uno sviluppatore archivia il contenuto nel progetto team. La definizione di compilazione esegue un file di progetto MSBuild personalizzato per distribuire pacchetti Web e script di database in un ambiente server di destinazione.

Affinché una distribuzione automatica abbia esito positivo come parte di un processo di compilazione, è necessario concedere le autorizzazioni appropriate all'account del servizio di compilazione nei server Web di destinazione e nel server di database di destinazione. Nell'ultimo argomento di questa esercitazione, [configurazione delle autorizzazioni per la distribuzione di Team Build](configuring-permissions-for-team-build-deployment.md), viene descritto come identificare e configurare le autorizzazioni necessarie per la distribuzione automatica da un server Team Build.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla creazione di definizioni di compilazione, vedere [creare una definizione di compilazione di base](https://msdn.microsoft.com/library/ms181716.aspx) e [definire il processo di compilazione](https://msdn.microsoft.com/library/ms181715.aspx). Per ulteriori informazioni sulle compilazioni di Accodamento, vedere [accodare una compilazione](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Precedente](configuring-a-tfs-build-server-for-web-deployment.md)
> [Successivo](deploying-a-specific-build.md)
