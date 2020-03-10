---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Configurazione della soluzione Contact Manager | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come scaricare e configurare la soluzione Contact Manager per l'esecuzione locale in una workstation per sviluppatori.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d9774ee01cb0515d7e733b24baa661f2648bd7c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629857"
---
# <a name="setting-up-the-contact-manager-solution"></a>Configurazione della soluzione Contact Manager

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come scaricare e configurare la soluzione Contact Manager per l'esecuzione locale in una workstation per sviluppatori.

## <a name="system-requirements"></a>Requisiti di sistema

Per eseguire la soluzione Contact Manager in locale e per eseguire le altre attività descritte in questa esercitazione, è necessario installare questo software nella workstation per sviluppatori:

- Visual Studio 2010 Service Pack 1, Premium o Ultimate Edition
- Internet Information Services (IIS) 7.5 Express
- SQL Server Express 2008 R2
- Strumento di distribuzione Web IIS (Distribuzione Web) 2,1 o versione successiva
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

Ad eccezione di Visual Studio 2010, è possibile scaricare e installare le versioni più recenti di tutti questi prodotti e componenti tramite l' [installazione guidata piattaforma Web](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Scaricare ed estrarre la soluzione

È possibile scaricare l'applicazione di esempio Contact Manager da MSDN Code Gallery [qui](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Configurare ed eseguire la soluzione

Per configurare ed eseguire la soluzione Contact Manager nel computer locale, è necessario eseguire i passaggi di alto livello seguenti:

1. Se non ne è già disponibile uno, creare un database locale di servizi delle applicazioni ASP.NET con le funzionalità di appartenenza e gestione dei ruoli abilitate.
2. Modificare le stringhe di connessione nei file *Web. config* in modo che puntino all'istanza di SQL Server Express locale.
3. Eseguire la soluzione da Visual Studio 2010.

Nella parte restante di questa sezione vengono fornite ulteriori indicazioni su come completare ognuna di queste attività.

**Per creare il database dei servizi delle applicazioni**

1. Aprire il prompt dei comandi di Visual Studio 2010. A tale scopo, fare clic sul pulsante **Start** , scegliere **tutti i programmi**, **Microsoft Visual Studio 2010**, fare clic su **strumenti di Visual Studio**, quindi su prompt dei **comandi di Visual Studio (2010)** .
2. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Usare l'opzione **– C** per specificare la stringa di connessione per il server di database.
    2. Utilizzare l'opzione **-** a per specificare le funzionalità dei servizi dell'applicazione che si desidera aggiungere al database. In questo caso, **m** indica che si desidera aggiungere il supporto per il provider di appartenenze e **r** indica che si desidera aggiungere il supporto per gestione ruoli.
    3. Usare l'opzione **– d** per specificare un nome per il database dei servizi dell'applicazione. Se si omette questa opzione, l'utilità creerà un database con il nome predefinito **aspnetdb**.
3. Quando il database è stato creato correttamente, il prompt dei comandi visualizzerà una conferma.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Per ulteriori informazioni sull'utilità ASPNET\_RegSql, vedere [ASP.NET SQL Server Registration Tool (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).

Il passaggio successivo consiste nel verificare che le stringhe di connessione nella soluzione Contact Manager puntino all'istanza locale di SQL Server Express.

**Per aggiornare le stringhe di connessione**

1. Aprire la soluzione Contact Manager in Visual Studio 2010.
2. Nella finestra di **Esplora soluzioni** espandere il progetto **ContactManager. Mvc** , quindi fare doppio clic sul nodo **Web. config** .

    > [!NOTE]
    > Il progetto ContactManager. MVC include due file *Web. config* . È necessario modificare il file a livello di progetto.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. Nell'elemento **connectionStrings** verificare che la stringa di connessione denominata **ApplicationServices** punti al database locale dei servizi dell'applicazione ASP.NET.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. Nella finestra di **Esplora soluzioni** espandere il progetto **ContactManager. Service** , quindi fare doppio clic sul nodo **Web. config** .

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. Nell'elemento **connectionStrings** , nella stringa di connessione denominata **ContactManagerContext**, verificare che la proprietà **origine dati** sia impostata sull'istanza locale di SQL Server Express. Non è necessario modificare nient'altro nella stringa di connessione.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Salvare tutti i file aperti.

A questo punto si è pronti per eseguire la soluzione Contact Manager nel computer locale.

> [!NOTE]
> Se si esegue questa procedura senza prima creare un database dei servizi delle applicazioni, ASP.NET creerà il database la prima volta che si tenta di creare un utente. Tuttavia, la creazione manuale del database offre un maggiore controllo sul set di funzionalità dei servizi applicazioni che si desidera supportare.

**Per eseguire la soluzione Contact Manager**

1. In Visual Studio 2010, premere F5.
2. Internet Explorer viene avviato e richiede l'URL dell'applicazione Contact Manager ASP.NET MVC 3. Per impostazione predefinita, l'applicazione Visualizza la pagina **tutti i contatti** .

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Aggiungere alcuni contatti, quindi verificare che l'applicazione funzioni come previsto.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Passare a `http://localhost:50114/Account/Register` (modificare l'URL se l'applicazione viene ospitata in una porta diversa). Aggiungere un nome utente, un indirizzo di posta elettronica e una password e verificare di essere in grado di registrare correttamente un account.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Passare a `http://localhost:50114/Account/LogOn` (modificare l'URL se l'applicazione viene ospitata in una porta diversa). Verificare che sia possibile accedere usando l'account appena creato.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Chiudere Internet Explorer per arrestare il debug.

## <a name="conclusion"></a>Conclusione

A questo punto, la soluzione Contact Manager deve essere completamente configurata per essere eseguita nel computer locale. È possibile utilizzare la soluzione come riferimento quando si utilizzano gli altri argomenti in questa esercitazione.

Nell'argomento successivo, [informazioni sul file di progetto](understanding-the-project-file.md), viene illustrato come è possibile utilizzare i file di progetto personalizzati Microsoft Build Engine (MSBuild) nella soluzione Contact Manager per controllare il processo di distribuzione.

> [!div class="step-by-step"]
> [Precedente](the-contact-manager-solution.md)
> [Successivo](understanding-the-project-file.md)
