---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Distribuzione delle appartenenze ai ruoli del database negli ambienti di test | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come aggiungere account utente ai ruoli del database come parte di una distribuzione di soluzione in un ambiente di testing. Quando si distribuisce una soluzione contenente...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a15f5bf5f659d151e91ef9e53c5ad55bcd8e2b01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587591"
---
# <a name="deploying-database-role-memberships-to-test-environments"></a>Distribuzione delle appartenenze ai ruoli del database negli ambienti di test

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come aggiungere account utente ai ruoli del database come parte di una distribuzione di soluzione in un ambiente di testing.
> 
> Quando si distribuisce una soluzione contenente un progetto di database in un ambiente di gestione temporanea o di produzione, in genere non si desidera che lo sviluppatore automatizza l'aggiunta di account utente ai ruoli del database. Nella maggior parte dei casi, lo sviluppatore non saprà quali account utente devono essere aggiunti ai ruoli del database e tali requisiti potrebbero cambiare in qualsiasi momento. Tuttavia, quando si distribuisce una soluzione contenente un progetto di database in un ambiente di sviluppo o di test, la situazione è generalmente piuttosto diversa:
> 
> - Lo sviluppatore in genere distribuisce di nuovo la soluzione a intervalli regolari, spesso diverse volte al giorno.
> - Il database viene in genere ricreato in ogni distribuzione, il che significa che gli utenti del database devono essere creati e aggiunti ai ruoli dopo ogni distribuzione.
> - Lo sviluppatore in genere ha il controllo completo sull'ambiente di sviluppo o di test di destinazione.
> 
> In questo scenario è spesso vantaggioso creare automaticamente gli utenti del database e assegnare le appartenenze ai ruoli del database come parte del processo di distribuzione.
> 
> Il fattore chiave è che questa operazione deve essere condizionale in base all'ambiente di destinazione. Se si esegue la distribuzione in un ambiente di gestione temporanea o di produzione, si desidera ignorare l'operazione. Se si esegue la distribuzione in un ambiente di sviluppo o di test, si desidera distribuire le appartenenze ai ruoli senza ulteriore intervento. In questo argomento viene descritto un approccio che è possibile utilizzare per risolvere questo problema.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica delle attività

In questo argomento si presuppone che l'utente:

- Usare l'approccio Split Project file per la distribuzione della soluzione, come descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Per distribuire il progetto di database, è possibile chiamare VSDBCMD dal file di progetto, come descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Per creare utenti di database e assegnare le appartenenze ai ruoli quando si distribuisce un progetto di database in un ambiente di testing, è necessario:

- Creare uno script Transact Structured Query Language (Transact-SQL) che apporta le modifiche necessarie al database.
- Creare una destinazione Microsoft Build Engine (MSBuild) che utilizza l'utilità sqlcmd. exe per eseguire lo script SQL.
- Configurare i file di progetto per richiamare la destinazione quando si distribuisce la soluzione in un ambiente di testing.

In questo argomento viene illustrato come eseguire ognuna di queste procedure.

## <a name="scripting-the-database-role-memberships"></a>Creazione di script per le appartenenze ai ruoli del database

È possibile creare uno script Transact-SQL in molti modi diversi e in qualsiasi posizione scelta. L'approccio più semplice consiste nel creare lo script all'interno della soluzione in Visual Studio 2010.

**Per creare uno script SQL**

1. Nella finestra **Esplora soluzioni** espandere il nodo del progetto di database.
2. Fare clic con il pulsante destro del mouse sulla cartella **script** , scegliere **Aggiungi**, quindi fare clic su **nuova cartella**.
3. Digitare **test** come nome della cartella, quindi premere INVIO.
4. Fare clic con il pulsante destro del mouse sulla cartella **test** , scegliere **Aggiungi**, quindi fare clic su **script**.
5. Nella finestra di dialogo **Aggiungi nuovo elemento** assegnare un nome significativo allo script (ad esempio, **AddRoleMemberships. SQL**), quindi fare clic su **Aggiungi**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. Nel file *AddRoleMemberships. SQL* aggiungere istruzioni Transact-SQL che:

    1. Creare un utente di database per l'account di accesso SQL Server che accederà al database.
    2. Aggiungere l'utente del database a tutti i ruoli del database richiesti.
7. Il file dovrebbe essere simile al seguente:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Salvare il file.

## <a name="executing-the-script-on-the-target-database"></a>Esecuzione dello script nel database di destinazione

Idealmente, è consigliabile eseguire tutti gli script Transact-SQL richiesti come parte di uno script di post-distribuzione quando si distribuisce il progetto di database. Gli script post-distribuzione, tuttavia, non consentono di eseguire la logica in modo condizionale in base alle configurazioni della soluzione o alle proprietà di compilazione. In alternativa, è possibile eseguire gli script SQL direttamente dal file di progetto MSBuild creando un elemento **target** che esegue un comando sqlcmd. exe. È possibile usare questo comando per eseguire lo script nel database di destinazione:

[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]

> [!NOTE]
> Per ulteriori informazioni sulle opzioni della riga di comando di sqlcmd, vedere [utilità sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

Prima di incorporare questo comando in una destinazione MSBuild, è necessario prendere in considerazione le condizioni in base alle quali si desidera eseguire lo script:

- Il database di destinazione deve esistere prima di modificare le appartenenze ai ruoli. Di conseguenza, è necessario eseguire questo script *dopo* la distribuzione del database.
- È necessario includere una condizione in modo che lo script venga eseguito solo per gli ambienti di test.
- Se in altre parole si esegue una distribuzione&#x2014;di tipo "simulazione", se si generano script di distribuzione, ma non&#x2014;è necessario eseguire lo script SQL.

Se si usa l'approccio per il file di progetto suddiviso descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), come illustrato nella soluzione di esempio Contact Manager, è possibile suddividere le istruzioni di compilazione per lo script SQL come segue:

- Eventuali proprietà specifiche dell'ambiente necessarie, insieme alla proprietà che determina se distribuire le autorizzazioni, devono essere inserite nel file di progetto specifico dell'ambiente (ad esempio, *env-dev. proj*).
- La destinazione MSBuild stessa, insieme a tutte le proprietà che non verranno modificate tra gli ambienti di destinazione, dovrebbe essere presente nel file di progetto universale (ad esempio, *Publish. proj*).

Nel file di progetto specifico dell'ambiente è necessario definire il nome del server di database, il nome del database di destinazione e una proprietà booleana che consente all'utente di specificare se distribuire le appartenenze ai ruoli.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]

Nel file di progetto universale è necessario specificare il percorso dell'eseguibile sqlcmd e il percorso dello script SQL che si desidera eseguire. Queste proprietà rimarranno invariate indipendentemente dall'ambiente di destinazione. È anche necessario creare una destinazione di MSBuild per eseguire il comando sqlcmd.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]

Si noti che si aggiunge il percorso del file eseguibile sqlcmd come proprietà statica, in quanto può essere utile ad altre destinazioni. Al contrario, si definisce la posizione dello script SQL e la sintassi del comando sqlcmd come proprietà dinamiche all'interno della destinazione, perché non saranno necessarie prima dell'esecuzione della destinazione. In questo caso, la destinazione **DeployTestDBPermissions** verrà eseguita solo se vengono soddisfatte le condizioni seguenti:

- La proprietà **DeployTestDBRoleMemberships** è impostata su **true**.
- L'utente non ha specificato un flag **whatIf = true** .

Infine, non dimenticare di richiamare la destinazione. Nel file *Publish. proj* è possibile eseguire questa operazione aggiungendo la destinazione all'elenco di dipendenze per la destinazione **FullPublish** predefinita. È necessario assicurarsi che la destinazione **DeployTestDBPermissions** non venga eseguita fino a quando non viene eseguita la destinazione **PublishDbPackages** .

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto un modo in cui è possibile aggiungere utenti del database e appartenenze ai ruoli come azione post-distribuzione quando si distribuisce un progetto di database. Questa operazione è in genere utile quando si ricrea regolarmente un database in un ambiente di test, ma in genere è consigliabile evitarlo quando si distribuiscono i database negli ambienti di gestione temporanea o di produzione. Di conseguenza, è necessario assicurarsi di utilizzare la logica condizionale necessaria in modo che gli utenti e le appartenenze ai ruoli del database vengano creati solo quando è appropriato.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sull'utilizzo di VSDBCMD per distribuire progetti di database, vedere [distribuzione di progetti di database](../web-deployment-in-the-enterprise/deploying-database-projects.md). Per indicazioni sulla personalizzazione delle distribuzioni di database per ambienti di destinazione diversi, vedere [personalizzazione delle distribuzioni di database per più ambienti](customizing-database-deployments-for-multiple-environments.md). Per ulteriori informazioni sull'utilizzo di file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Per ulteriori informazioni sulle opzioni della riga di comando di sqlcmd, vedere [utilità sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Precedente](customizing-database-deployments-for-multiple-environments.md)
> [Successivo](deploying-membership-databases-to-enterprise-environments.md)
