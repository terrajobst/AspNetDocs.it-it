---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Distribuzione Web ASP.NET con Visual Studio: impostazione delle autorizzazioni per le cartelle | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 410525bb2e3f6e5a0be6d7d6b33fb3a40509041a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614930"
---
# <a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Distribuzione Web ASP.NET con Visual Studio: impostazione delle autorizzazioni per le cartelle

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica di

In questa esercitazione si impostano le autorizzazioni per la cartella *ELMAH* nel sito Web distribuito in modo che l'applicazione possa creare file di log in tale cartella.

Quando si esegue il test di un'applicazione Web in Visual Studio usando il Server di sviluppo Visual Studio (Cassini) o IIS Express, l'applicazione viene eseguita con l'identità. È molto probabile che un amministratore del computer di sviluppo disponga di un'autorità completa per eseguire qualsiasi operazione su qualsiasi file in qualsiasi cartella. Tuttavia, quando un'applicazione viene eseguita in IIS, viene eseguita con l'identità definita per il pool di applicazioni a cui è assegnato il sito. Si tratta in genere di un account definito dal sistema che dispone di autorizzazioni limitate. Per impostazione predefinita, dispone delle autorizzazioni di lettura ed esecuzione per i file e le cartelle dell'applicazione Web, ma non dispone dell'accesso in scrittura.

Questo diventa un problema se l'applicazione crea o aggiorna i file, una necessità comune nelle applicazioni Web. Nell'applicazione Contoso University, ELMAH crea file XML nella cartella *ELMAH* per salvare i dettagli sugli errori. Anche se non si usa un elemento come ELMAH, il sito potrebbe consentire agli utenti di caricare i file o eseguire altre attività che scrivono i dati in una cartella nel sito.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Verifica della registrazione degli errori e della creazione di report

Per verificare il funzionamento corretto dell'applicazione in IIS (anche se è stato testato in Visual Studio), è possibile che venga generato un errore che in genere verrebbe registrato da ELMAH, quindi aprire il log degli errori di ELMAH per visualizzare i dettagli. Se ELMAH non è stato in grado di creare un file XML e archivia i dettagli dell'errore, viene visualizzata una segnalazione di errore vuota.

Aprire un browser e passare a `http://localhost/ContosoUniversity`e quindi richiedere un URL non valido, ad esempio *Studentsxxx. aspx*. Viene visualizzata una pagina di errore generata dal sistema anziché la pagina *pagina GenericErrorPage. aspx* perché l'impostazione `customErrors` nel file Web. config è "RemoteOnly" e si esegue IIS in locale:

![Pagina di errore HTTP 404](setting-folder-permissions/_static/image1.png)

Eseguire quindi *ELMAH. axd* per visualizzare il report degli errori. Dopo aver eseguito l'accesso con le credenziali dell'account amministratore (&quot;amministratore&quot; e &quot;devpwd&quot;), viene visualizzata una pagina di log degli errori vuota perché ELMAH non è stato in grado di creare un file XML nella cartella *ELMAH* :

![Log degli errori vuoto](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Impostare l'autorizzazione di scrittura per la cartella ELMAH

È possibile impostare le autorizzazioni della cartella manualmente oppure è possibile renderla automaticamente parte del processo di distribuzione. Per renderlo automatico è necessario un codice MSBuild complesso e, dal momento che è necessario eseguire questa operazione solo la prima volta che si distribuisce, la procedura seguente illustra come eseguire questa operazione manualmente. Per informazioni su come eseguire questa parte del processo di distribuzione, vedere [impostazione delle autorizzazioni per le cartelle sul Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) nel Blog di Sayed Hashimi.

1. In **Esplora file**passare a *C:\inetpub\wwwroot\ContosoUniversity*. Fare clic con il pulsante destro del mouse sulla cartella *ELMAH* , scegliere **Proprietà**, quindi selezionare la scheda **sicurezza** .
2. Fare clic su **Modifica**.
3. Nella finestra di dialogo **autorizzazioni per ELMAH** selezionare **DefaultAppPool**, quindi selezionare la casella di controllo **Scrivi** nella colonna **Consenti** .

    ![Autorizzazioni per la cartella ELMAH](setting-folder-permissions/_static/image3.png)

    Se non viene visualizzato **DefaultAppPool** nell'elenco di **utenti o gruppi** , è probabile che sia stato usato un altro metodo rispetto a quello specificato in questa esercitazione per configurare IIS e ASP.NET 4 nel computer. In tal caso, scoprire quale identità viene usata dal pool di applicazioni assegnato all'applicazione Contoso University e concedere l'autorizzazione di scrittura per tale identità. Vedere i collegamenti sulle identità del pool di applicazioni alla fine di questa esercitazione. Fare clic su **OK** in entrambe le finestre di dialogo.

## <a name="retest-error-logging-and-reporting"></a>Testare nuovamente la registrazione degli errori e la creazione di report

Eseguire un test causando un errore nello stesso modo (richiedere un URL non valido) ed eseguire la pagina **log degli errori** . Questa volta l'errore viene visualizzato nella pagina.

![Pagina log degli errori di ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Riepilogo

Sono state completate tutte le attività necessarie per far funzionare correttamente Contoso University in IIS nel computer locale. Nell'esercitazione successiva il sito sarà reso disponibile pubblicamente tramite la distribuzione in Azure.

## <a name="more-information"></a>Altre informazioni

In questo esempio, il motivo per cui ELMAH non è stato in grado di salvare i file di log era abbastanza ovvio. È possibile utilizzare la traccia IIS nei casi in cui la causa del problema non è così ovvia; vedere [risoluzione dei problemi relativi alle richieste non riuscite tramite la traccia in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) nel sito di IIS.NET.

Per altre informazioni su come concedere le autorizzazioni alle identità del pool di applicazioni, vedere [identità del pool di applicazioni](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [contenuto sicuro in IIS tramite ACL del file System](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) nel sito di IIS.NET.

> [!div class="step-by-step"]
> [Precedente](deploying-to-iis.md)
> [Successivo](deploying-to-production.md)
