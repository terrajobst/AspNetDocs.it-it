---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Esclusione di file e cartelle dalla distribuzione | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come escludere file e cartelle da un pacchetto di distribuzione Web quando si compila e si crea il pacchetto di un progetto di applicazione Web.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: a262ce43d7199fb1015d54d0b7c213857c360946
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544975"
---
# <a name="excluding-files-and-folders-from-deployment"></a>Esclusione di file e cartelle dalla distribuzione

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come escludere file e cartelle da un pacchetto di distribuzione Web quando si compila e si crea il pacchetto di un progetto di applicazione Web.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="overview"></a>Panoramica

Quando si compila un progetto di applicazione Web in Visual Studio 2010, la pipeline di pubblicazione Web (WPP) consente di estendere il processo di compilazione compilando l'applicazione Web compilata in un pacchetto web distribuibile. È quindi possibile utilizzare lo strumento di distribuzione Web Internet Information Services (IIS) (Distribuzione Web) per distribuire il pacchetto Web in un server Web IIS remoto o importare il pacchetto Web manualmente tramite Gestione IIS. Questo processo di creazione dei pacchetti è illustrato in [creazione e creazione di pacchetti di progetti di applicazione Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

In che modo è possibile controllare ciò che viene incluso nel pacchetto Web? Le impostazioni del progetto in Visual Studio, tramite il file di progetto sottostante, forniscono un controllo sufficiente per diversi scenari. In alcuni casi, tuttavia, potrebbe essere necessario personalizzare il contenuto del pacchetto Web in ambienti di destinazione specifici. Ad esempio, potrebbe essere necessario includere una cartella per i file di log quando si distribuisce l'applicazione in un ambiente di testing, ma si esclude la cartella quando si distribuisce l'applicazione in un ambiente di gestione temporanea o di produzione. In questo argomento viene illustrato come eseguire questa operazione.

## <a name="what-gets-included-by-default"></a>Che cosa viene incluso per impostazione predefinita?

Quando si configurano le proprietà del progetto di applicazione Web in Visual Studio, l'elenco **elementi da distribuire** nella pagina **Web del pacchetto/pubblicazione** consente di specificare ciò che si desidera includere nel pacchetto di distribuzione Web. Per impostazione predefinita, questa opzione è impostata su **solo i file necessari per eseguire l'applicazione**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Quando si scelgono **solo i file necessari per eseguire l'applicazione**, il WPP tenterà di determinare i file da aggiungere al pacchetto Web. vale a dire:

- Tutti gli output di compilazione per il progetto.
- Tutti i file contrassegnati con un'azione di compilazione del **contenuto**.

> [!NOTE]
> La logica che determina i file da includere è contenuta in questo file:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft. Web. Publishing. OnlyFilesToRunTheApp. targets*

## <a name="excluding-specific-files-and-folders"></a>Esclusione di file e cartelle specifici

In alcuni casi, è necessario un controllo più granulare sui file e sulle cartelle che vengono distribuiti. Se si conoscono i file che si desidera escludere in anticipo e l'esclusione si applica a tutti gli ambienti di destinazione, è possibile impostare semplicemente l' **azione di compilazione** di ogni file su **nessuno**.

**Per escludere file specifici dalla distribuzione**

1. Nella finestra di **Esplora soluzioni** , fare clic con il pulsante destro del mouse sul file, quindi scegliere **Proprietà**.
2. Nella riga **azione di compilazione** della finestra **Proprietà** selezionare **nessuno**.

Tuttavia, questo approccio non è sempre pratico. È possibile, ad esempio, modificare i file e le cartelle inclusi in base all'ambiente di destinazione e all'esterno di Visual Studio. Ad esempio, nella soluzione di esempio Contact Manager esaminare il contenuto del progetto ContactManager. Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- La cartella interna contiene alcuni script SQL utilizzati dallo sviluppatore per creare, eliminare e popolare i database locali a scopo di sviluppo. Nessun elemento in questa cartella deve essere distribuito in un ambiente di gestione temporanea o di produzione.
- La cartella Scripts contiene diversi file JavaScript. Numerosi file sono inclusi esclusivamente per supportare il debug o fornire IntelliSense in Visual Studio. Alcuni di questi file non devono essere distribuiti negli ambienti di gestione temporanea o di produzione. Tuttavia, potrebbe essere necessario distribuirli in un ambiente di test per sviluppatori per semplificare la risoluzione dei problemi.

Sebbene sia possibile modificare i file di progetto in modo da escludere file e cartelle specifici, è più semplice. Il WPP include un meccanismo per escludere file e cartelle tramite la compilazione di elenchi di elementi denominati **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles**. È possibile estendere questo meccanismo aggiungendo elementi personalizzati a questi elenchi. A tale scopo, è necessario completare questi passaggi di alto livello:

1. Creare un file di progetto personalizzato denominato *[nome progetto]. WPP. targets* nella stessa cartella del file di progetto.

    > [!NOTE]
    > Il file *. WPP. targets* deve essere inserito nella stessa cartella del file&#x2014;di progetto dell'applicazione Web, ad esempio *ContactManager. Mvc. csproj*&#x2014;anziché nella stessa cartella dei file di progetto personalizzati usati per controllare il processo di compilazione e distribuzione.
2. Nel file con *estensione WPP. targets* aggiungere un elemento **ItemGroup** .
3. Nell'elemento **ItemGroup** aggiungere gli elementi **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles** per escludere file e cartelle specifici in base alle esigenze.

Questa è la struttura di base del file con estensione *WPP. targets* :

[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]

Si noti che ogni elemento include un elemento dei metadati dell'elemento denominato **FromTarget**. Si tratta di un valore facoltativo che non influisce sul processo di compilazione. serve semplicemente a indicare perché determinati file o cartelle sono stati omessi se qualcuno esamina i log di compilazione.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Esclusione di file e cartelle da un pacchetto Web

La procedura seguente illustra come aggiungere un file con estensione *WPP. targets* a un progetto di applicazione Web e come usare il file per escludere file e cartelle specifici dal pacchetto Web quando si compila il progetto.

**Per escludere file e cartelle da un pacchetto di distribuzione Web**

1. Aprire la soluzione in Visual Studio 2010.
2. Nella finestra di **Esplora soluzioni** , fare clic con il pulsante destro del mouse sul nodo del progetto di applicazione Web (ad esempio, **ContactManager. Mvc**), scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.
3. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello di **file XML** .
4. Nella casella **nome** Digitare *[nome progetto] * * *. WPP. targets** (ad esempio, **ContactManager. Mvc. WPP. targets**), quindi fare clic su **Aggiungi**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Se si aggiunge un nuovo elemento al nodo radice di un progetto, il file viene creato nella stessa cartella del file di progetto. È possibile verificarlo aprendo la cartella in Esplora risorse.
5. Nel file aggiungere un elemento **Project** e un elemento **ItemGroup** :

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Se si desidera escludere le cartelle dal pacchetto Web, aggiungere un elemento **ExcludeFromPackageFolders** all'elemento **ItemGroup** :

   1. Nell'attributo **include** specificare un elenco delimitato da punti e virgola delle cartelle che si desidera escludere.
   2. Nell'elemento dei metadati **FromTarget** , fornire un valore significativo per indicare il motivo per cui vengono escluse le cartelle, ad esempio il nome del file con *estensione WPP. targets* .

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Se si desidera escludere i file dal pacchetto Web, aggiungere un elemento **ExcludeFromPackageFiles** all'elemento **ItemGroup** :

   1. Nell'attributo **include** specificare un elenco delimitato da punti e virgola dei file che si desidera escludere.
   2. Nell'elemento dei metadati **FromTarget** , fornire un valore significativo per indicare il motivo per cui vengono esclusi i file, ad esempio il nome del file con *estensione WPP. targets* .

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. Il file *[nome progetto]. WPP. targets* dovrebbe essere simile al seguente:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Salvare e chiudere il file *[nome progetto]. WPP. targets* .

La volta successiva che si compila e si crea il pacchetto del progetto di applicazione Web, WPP rileverà automaticamente il file con *estensione WPP. targets* . Tutti i file e le cartelle specificati non verranno inclusi nel pacchetto Web.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come escludere file e cartelle specifici quando si compila un pacchetto Web creando un file con *estensione WPP. targets* personalizzato nella stessa cartella del file di progetto dell'applicazione Web.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sull'utilizzo di file di progetto personalizzati Microsoft Build Engine (MSBuild) per controllare il processo di distribuzione, vedere [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Per ulteriori informazioni sul processo di creazione di pacchetti e distribuzione, vedere [compilazione e creazione](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)di pacchetti di progetti di applicazioni Web, [configurazione di parametri per la distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)e [distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Precedente](deploying-membership-databases-to-enterprise-environments.md)
> [Successivo](taking-web-applications-offline-with-web-deploy.md)
