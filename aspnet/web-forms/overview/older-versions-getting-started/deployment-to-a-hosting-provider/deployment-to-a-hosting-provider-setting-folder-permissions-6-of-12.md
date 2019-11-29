---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: impostazione delle autorizzazioni per le cartelle-6 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 85a77a196cf3458bbb2e6308838a846936cd070b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633583"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: impostazione delle autorizzazioni per le cartelle-6 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in app Azure app Web del servizio, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Panoramica di

In questa esercitazione si impostano le autorizzazioni per la cartella *ELMAH* nel sito Web distribuito in modo che l'applicazione possa creare file di log in tale cartella.

Quando si esegue il test di un'applicazione Web in Visual Studio usando il Server di sviluppo Visual Studio (Cassini), l'applicazione viene eseguita con l'identità. È molto probabile che un amministratore del computer di sviluppo disponga di un'autorità completa per eseguire qualsiasi operazione su qualsiasi file in qualsiasi cartella. Tuttavia, quando un'applicazione viene eseguita in IIS, viene eseguita con l'identità definita per il pool di applicazioni a cui è assegnato il sito. Si tratta in genere di un account definito dal sistema che dispone di autorizzazioni limitate. Per impostazione predefinita, dispone delle autorizzazioni di lettura ed esecuzione per i file e le cartelle dell'applicazione Web, ma non dispone dell'accesso in scrittura.

Questo diventa un problema se l'applicazione crea o aggiorna i file, una necessità comune nelle applicazioni Web. Nell'applicazione Contoso University, ELMAH crea file XML nella cartella *ELMAH* per salvare i dettagli sugli errori. Anche se non si usa un elemento come ELMAH, il sito potrebbe consentire agli utenti di caricare i file o eseguire altre attività che scrivono i dati in una cartella nel sito.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Verifica della registrazione e della creazione di report degli errori

Per verificare il funzionamento corretto dell'applicazione in IIS (anche se è stato testato in Visual Studio), è possibile che venga generato un errore che in genere verrebbe registrato da ELMAH, quindi aprire il log degli errori di ELMAH per visualizzare i dettagli. Se ELMAH non è stato in grado di creare un file XML e archivia i dettagli dell'errore, viene visualizzata una segnalazione di errore vuota.

Aprire un browser e passare a `http://localhost/ContosoUniversity`e quindi richiedere un URL non valido, ad esempio *Studentsxxx. aspx*. Viene visualizzata una pagina di errore generata dal sistema anziché la pagina *pagina GenericErrorPage. aspx* perché l'impostazione `customErrors` nel file Web. config è "RemoteOnly" e si esegue IIS in locale:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Eseguire quindi *ELMAH. axd* per visualizzare il report degli errori. Viene visualizzata una pagina di log degli errori vuota perché ELMAH non è stato in grado di creare un file XML nella cartella *ELMAH* :

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Impostazione dell'autorizzazione di scrittura per la cartella ELMAH

È possibile impostare le autorizzazioni della cartella manualmente oppure è possibile renderla automaticamente parte del processo di distribuzione. Per renderlo automatico è necessario un codice MSBuild complesso e, dal momento che è necessario eseguire questa operazione solo la prima volta che si distribuisce, questa esercitazione Mostra solo come eseguire questa operazione manualmente. Per informazioni su come eseguire questa parte del processo di distribuzione, vedere [impostazione delle autorizzazioni per le cartelle sul Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) nel Blog di Sayed Hashimi.

In **Esplora risorse**passare a *C:\inetpub\wwwroot\ContosoUniversity*. Fare clic con il pulsante destro del mouse sulla cartella *ELMAH* , scegliere **Proprietà**, quindi selezionare la scheda **sicurezza** .

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

Se non viene visualizzato **DefaultAppPool** nell'elenco di **utenti o gruppi** , è probabile che sia stato usato un altro metodo rispetto a quello specificato in questa esercitazione per configurare IIS e ASP.NET 4 nel computer. In tal caso, scoprire quale identità viene usata dal pool di applicazioni assegnato all'applicazione Contoso University e concedere l'autorizzazione di scrittura per tale identità. Vedere i collegamenti sulle identità del pool di applicazioni alla fine di questa esercitazione.

Fare clic su **Modifica**. Nella finestra di dialogo **autorizzazioni per ELMAH** selezionare **DefaultAppPool**, quindi selezionare la casella di controllo **Scrivi** nella colonna **Consenti** .

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Fare clic su **OK** in entrambe le finestre di dialogo.

## <a name="retesting-error-logging-and-reporting"></a>Verifica della registrazione degli errori e della creazione di report

Eseguire un test causando un errore nello stesso modo (richiedere un URL non valido) ed eseguire la pagina **log degli errori** . Questa volta l'errore viene visualizzato nella pagina.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

È anche necessaria l'autorizzazione di scrittura per la cartella *App\_data* perché sono presenti SQL Server Compact file di database in tale cartella e si desidera essere in grado di aggiornare i dati in tali database. In tal caso, tuttavia, non è necessario eseguire altre operazioni, perché il processo di distribuzione imposta automaticamente l'autorizzazione di scrittura per la cartella *App\_data* .

Sono state completate tutte le attività necessarie per far funzionare correttamente Contoso University in IIS nel computer locale. Nell'esercitazione successiva si renderà disponibile pubblicamente il sito distribuendo il sito a un provider di hosting.

## <a name="more-information"></a>Altre informazioni

In questo esempio, il motivo per cui ELMAH non è stato in grado di salvare i file di log era abbastanza ovvio. È possibile utilizzare la traccia IIS nei casi in cui la causa del problema non è così ovvia; vedere [risoluzione dei problemi relativi alle richieste non riuscite tramite la traccia in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) nel sito di IIS.NET.

Per altre informazioni su come concedere le autorizzazioni alle identità del pool di applicazioni, vedere [identità del pool di applicazioni](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [contenuto sicuro in IIS tramite ACL del file System](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) nel sito di IIS.NET.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
