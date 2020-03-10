---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Personalizzazione delle distribuzioni di database per più ambienti | Microsoft Docs
author: jrjlee
description: "In questo argomento viene descritto come personalizzare le proprietà di un database in ambienti di destinazione specifici come parte del processo di distribuzione. Nota: nell'argomento si presuppone..."
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 8ae8cb1a322afb95c5d2e8d5e73c7825c7b2fe5a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78604027"
---
# <a name="customizing-database-deployments-for-multiple-environments"></a>Personalizzazione delle distribuzioni di database per più ambienti

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come personalizzare le proprietà di un database in ambienti di destinazione specifici come parte del processo di distribuzione.
> 
> > [!NOTE]
> > Nell'argomento si presuppone che si stia distribuendo un progetto di database di Visual Studio 2010 usando MSBuild. exe e VSDBCMD. exe. Per ulteriori informazioni sui motivi per cui è possibile scegliere questo approccio, vedere [distribuzione Web nell'organizzazione](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) e [distribuzione di progetti di database](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Quando si distribuisce un progetto di database in più destinazioni, è spesso necessario personalizzare le proprietà di distribuzione del database per ogni ambiente di destinazione. Negli ambienti di test, ad esempio, è in genere necessario ricreare il database in ogni distribuzione, mentre negli ambienti di gestione temporanea o di produzione è molto più probabile eseguire aggiornamenti incrementali per conservare i dati.
> 
> In un progetto di database Visual Studio 2010, le impostazioni di distribuzione sono contenute in un file di configurazione della distribuzione (. sqldeployment). In questo argomento viene illustrato come creare file di configurazione della distribuzione specifici dell'ambiente e specificare quello che si desidera utilizzare come parametro VSDBCMD.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica delle attività

In questo argomento si presuppone che l'utente:

- Usare l'approccio Split Project file per la distribuzione della soluzione, come descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Per distribuire il progetto di database, è possibile chiamare VSDBCMD dal file di progetto, come descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Per creare un sistema di distribuzione che supporti la variazione delle proprietà di distribuzione del database tra gli ambienti di destinazione, è necessario:

- Creare un file di configurazione della distribuzione (. sqldeployment) per ogni ambiente di destinazione.
- Creare un comando VSDBCMD che specifichi il file di configurazione della distribuzione come opzione della riga di comando.
- Parametrizzare il comando VSDBCMD in un file di progetto Microsoft Build Engine (MSBuild), in modo che le opzioni VSDBCMD siano appropriate per l'ambiente di destinazione.

In questo argomento viene illustrato come eseguire ognuna di queste procedure.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Creazione di file di configurazione della distribuzione specifici dell'ambiente

Per impostazione predefinita, un progetto di database contiene un unico file di configurazione della distribuzione denominato *database. sqldeployment*. Se si apre il file in Visual Studio 2010, è possibile visualizzare le diverse opzioni di distribuzione disponibili:

- **Regole**di confronto per il confronto della distribuzione. Ciò consente di scegliere se utilizzare le regole di confronto del database del progetto (regole di confronto di *origine* ) o le regole di confronto del database del server di destinazione (regole di confronto di *destinazione* ). Nella maggior parte dei casi, è consigliabile utilizzare le regole di confronto di origine quando si esegue la distribuzione in un ambiente di sviluppo o di test. Quando si esegue la distribuzione in un ambiente di gestione temporanea o di produzione, in genere si desidera lasciare invariate le regole di confronto di destinazione per evitare eventuali problemi di interoperabilità.
- **Distribuire le proprietà del database**. Ciò consente di scegliere se applicare le proprietà del database, come definito nel file *database. sqlsettings* . Quando si distribuisce un database per la prima volta, è necessario distribuire le proprietà del database. Se si sta aggiornando un database esistente, le proprietà devono essere già presenti e non è necessario distribuirle nuovamente.
- **Ricreare sempre il database**. Ciò consente di scegliere se ricreare il database di destinazione ogni volta che si distribuiscono o si apportano modifiche incrementali per rendere aggiornato il database di destinazione con lo schema. Se si ricrea il database, si perderanno tutti i dati nel database esistente. Di conseguenza, in genere è necessario impostare su **false** per le distribuzioni in ambienti di gestione temporanea o di produzione.
- **Blocca la distribuzione incrementale se possono verificarsi perdite di dati**. Ciò consente di scegliere se la distribuzione deve essere interrotta se una modifica allo schema del database provocherà la perdita di dati. In genere, questo valore viene impostato su **true** per una distribuzione in un ambiente di produzione, per offrire l'opportunità di intervenire e proteggere i dati importanti. Se è stato impostato **ricrea sempre database** su **false**, questa impostazione non avrà alcun effetto.
- **Eseguire la distribuzione in modalità utente singolo**. Non si tratta in genere di un problema negli ambienti di sviluppo o di test. Tuttavia, in genere è necessario impostare su **true** per le distribuzioni in ambienti di gestione temporanea o di produzione. In questo modo si impedisce agli utenti di apportare modifiche al database mentre è in corso la distribuzione.
- **Eseguire il backup del database prima della distribuzione**. In genere, questo valore viene impostato su **true** quando si esegue la distribuzione in un ambiente di produzione, come precauzione dalla perdita di dati. È inoltre possibile impostarlo su **true** quando si esegue la distribuzione in un ambiente di gestione temporanea, se il database di gestione temporanea contiene una grande quantità di dati.
- **Genera istruzioni DROP per gli oggetti presenti nel database di destinazione ma che non sono presenti nel progetto di database**. Nella maggior parte dei casi, si tratta di una parte integrante e essenziale di apportare modifiche incrementali a un database. Se è stato impostato **ricrea sempre database** su **false**, questa impostazione non avrà alcun effetto.
- **Non utilizzare istruzioni ALTER assembly per aggiornare i tipi CLR**. Questa impostazione determina il modo in cui SQL Server aggiornare i tipi di Common Language Runtime (CLR) a versioni di assembly più recenti. Questa impostazione deve essere impostata su **false** nella maggior parte degli scenari.

Questa tabella mostra le impostazioni di distribuzione tipiche per ambienti di destinazione diversi. Tuttavia, le impostazioni possono variare a seconda dei requisiti specifici.

|  | Developer/test | Gestione temporanea/integrazione | Produzione |
| --- | --- | --- | --- |
| **Regole di confronto per il confronto della distribuzione** | Origine | Destinazione | Destinazione |
| **Distribuisci proprietà database** | True | Solo per la prima volta | Solo per la prima volta |
| **Ricrea sempre database** | True | False | False |
| **Blocca la distribuzione incrementale se possono verificarsi perdite di dati** | False | È possibile | True |
| **Eseguire lo script di distribuzione in modalità utente singolo** | False | True | True |
| **Eseguire il backup del database prima della distribuzione** | False | È possibile | True |
| **Genera istruzioni DROP per gli oggetti presenti nel database di destinazione ma che non sono presenti nel progetto di database** | False | True | True |
| **Non utilizzare istruzioni ALTER ASSEMBLY per aggiornare i tipi CLR** | False | False | False |

> [!NOTE]
> Per ulteriori informazioni sulle proprietà di distribuzione del database e sulle considerazioni sull'ambiente, vedere [una panoramica delle impostazioni del progetto di database](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [procedura: configurare le proprietà per i dettagli della distribuzione](https://msdn.microsoft.com/library/dd172125.aspx), [compilare e distribuire il database in un ambiente di sviluppo isolato](https://msdn.microsoft.com/library/dd193409.aspx)e [compilare e distribuire database in un ambiente di gestione temporanea o di produzione](https://msdn.microsoft.com/library/dd193413.aspx).

Per supportare la distribuzione di un progetto di database in più destinazioni, è necessario creare un file di configurazione della distribuzione per ogni ambiente di destinazione.

**Per creare un file di configurazione specifico dell'ambiente**

1. In Visual Studio 2010, nella finestra di **Esplora soluzioni** , fare clic con il pulsante destro del mouse sul progetto di database e quindi scegliere **Proprietà**.
2. Nella scheda **Distribuisci** della pagina proprietà del progetto di database, nella riga **file di configurazione della distribuzione** , fare clic su **nuovo**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. Nella finestra di dialogo **nuovo file di configurazione della distribuzione** assegnare al file un nome significativo, ad esempio **TestEnvironment. sqldeployment**, quindi fare clic su **Salva**.
4. Nella pagina *[nomefile] * * *. sqldeployment** impostare le proprietà di distribuzione in modo che corrispondano ai requisiti dell'ambiente di destinazione, quindi salvare il file.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Si noti che il nuovo file viene aggiunto alla cartella Properties nel progetto di database.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Specifica del file di configurazione della distribuzione in VSDBCMD

Quando si usano configurazioni di soluzione, ad esempio debug e versione, all'interno di Visual Studio 2010, è possibile associare un file di configurazione della distribuzione a ogni configurazione. Quando si compila una configurazione specifica, il processo di compilazione genera un file manifesto di distribuzione specifico per la configurazione che punta al file di configurazione della distribuzione specifico della configurazione. Tuttavia, uno degli obiettivi principali dell'approccio alla distribuzione descritto in queste esercitazioni è fornire agli utenti la possibilità di controllare il processo di distribuzione senza usare Visual Studio 2010 e le configurazioni della soluzione. In questo approccio, la configurazione della soluzione è la stessa indipendentemente dall'ambiente di distribuzione di destinazione. Per personalizzare la distribuzione del database in un ambiente di destinazione specifico, è possibile utilizzare le opzioni della riga di comando VSDBCMD per specificare il file di configurazione della distribuzione.

Per specificare un file di configurazione della distribuzione in VSDBCMD, usare l'opzione **p:/DeploymentConfigurationFile** e specificare il percorso completo del file. Verrà eseguito l'override del file di configurazione della distribuzione identificato dal manifesto di distribuzione. Ad esempio, è possibile usare questo comando VSDBCMD per distribuire il database **ContactManager** in un ambiente di testing:

[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]

> [!NOTE]
> Si noti che il processo di compilazione può rinominare il file. sqldeployment quando copia il file nella directory di output.

Se si usano variabili di comando SQL negli script SQL di pre-distribuzione o post-distribuzione, è possibile usare un approccio simile per associare un file con estensione sqlcmdvars specifico dell'ambiente alla distribuzione. In questo caso, si usa l'opzione **p:/SqlCommandVariablesFile** per identificare il file. sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Esecuzione del comando VSDBCMD da un file di progetto MSBuild

È possibile richiamare un comando VSDBCMD da un file di progetto MSBuild utilizzando un'attività **Exec** all'interno di una destinazione MSBuild. Nella sua forma più semplice, avrà un aspetto simile al seguente:

[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]

- In pratica, per facilitare la lettura e il riutilizzo dei file di progetto, è opportuno creare proprietà per archiviare i vari parametri della riga di comando. Questo consente agli utenti di fornire più facilmente i valori delle proprietà in un file di progetto specifico dell'ambiente o di eseguire l'override dei valori predefiniti dalla riga di comando di MSBuild. Se si usa l'approccio Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), è necessario dividere le istruzioni e le proprietà di compilazione tra i due file di conseguenza:
- Le impostazioni specifiche dell'ambiente, ad esempio il nome del file di configurazione della distribuzione, la stringa di connessione del database e il nome del database di destinazione, devono essere inserite nel file di progetto specifico dell'ambiente.
- La destinazione MSBuild che esegue il comando VSDBCMD, insieme a qualsiasi proprietà universale come il percorso del file eseguibile VSDBCMD, deve essere presente nel file di progetto universale.

È inoltre necessario assicurarsi di compilare il progetto di database prima di richiamare VSDBCMD, in modo che il file con estensione deploymanifest venga creato e pronto per l'utilizzo. È possibile vedere un esempio completo di questo approccio nell'argomento [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), che illustra i file di progetto nella soluzione di [esempio Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come personalizzare le proprietà del database in ambienti di destinazione diversi quando si distribuiscono progetti di database tramite MSBuild e VSDBCMD. Questo approccio è utile quando è necessario distribuire progetti di database come parte di soluzioni di livello aziendale di dimensioni maggiori. Queste soluzioni vengono spesso distribuite in più destinazioni, ad esempio ambienti di sviluppo o di test in modalità sandbox, piattaforme di gestione temporanea o di integrazione, nonché ambienti di produzione o dinamici. Ognuno di questi ambienti di destinazione richiede in genere un set univoco di proprietà di distribuzione del database.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla distribuzione di progetti di database mediante VSDBCMD. exe, vedere [distribuzione di progetti di database](../web-deployment-in-the-enterprise/deploying-database-projects.md). Per ulteriori informazioni sull'utilizzo di file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Questi articoli su MSDN forniscono indicazioni più generali sulla distribuzione di database:

- [Panoramica delle impostazioni del progetto di database](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Procedura: configurare le proprietà per i dettagli della distribuzione](https://msdn.microsoft.com/library/dd172125.aspx)
- [Creare e distribuire database in un ambiente di sviluppo isolato](https://msdn.microsoft.com/library/dd193409.aspx)
- [Creare e distribuire database in un ambiente di gestione temporanea o di produzione](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Precedente](performing-a-what-if-deployment.md)
> [Successivo](deploying-database-role-memberships-to-test-environments.md)
