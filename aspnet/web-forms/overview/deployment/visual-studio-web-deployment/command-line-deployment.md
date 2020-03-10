---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Distribuzione Web ASP.NET con Visual Studio: distribuzione da riga di comando | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630921"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Distribuzione Web ASP.NET con Visual Studio: distribuzione da riga di comando

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica

Questa esercitazione illustra come richiamare la pipeline di pubblicazione Web di Visual Studio dalla riga di comando. Questa operazione è utile per gli scenari in cui si vuole [automatizzare il processo di distribuzione](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) anziché eseguirlo manualmente in Visual Studio, in genere usando un [sistema di controllo della versione del codice sorgente](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Apportare una modifica a deploy

Attualmente nella pagina about viene visualizzato il codice del modello.

![Pagina about con codice modello](command-line-deployment/_static/image1.png)

Si sostituirà con il codice che visualizza un riepilogo della registrazione studente.

Aprire la pagina *About. aspx* , eliminare tutto il markup all'interno dell'elemento `MainContent` `Content` e inserire il markup seguente al suo posto:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Eseguire il progetto e selezionare la pagina **informazioni su** .

![Pagina About (Informazioni)](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Distribuire per eseguire il test tramite la riga di comando

Non verrà distribuita un'altra modifica del database. disabilitare quindi la distribuzione del database dbDacFx per il database ASPNET-ContosoUniversity. Aprire la procedura guidata **Pubblica sito Web** e in ognuno dei tre profili di pubblicazione deselezionare la casella di controllo **Aggiorna database** nella scheda **Impostazioni** .

Nella pagina iniziale di Windows 8 cercare **prompt dei comandi per gli sviluppatori per VS2012**.

Fare clic con il pulsante destro del mouse sull'icona **prompt dei comandi per gli sviluppatori per VS2012** e scegliere **Esegui come amministratore**.

Al prompt dei comandi immettere il comando seguente, sostituendo il percorso del file di soluzione con il percorso del file di soluzione:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild compila la soluzione e la distribuisce nell'ambiente di test.

![Output della riga di comando](command-line-deployment/_static/image3.png)

Aprire un browser e passare a `http://localhost/ContosoUniversity`, quindi fare clic sulla pagina **informazioni** per verificare che la distribuzione sia stata completata correttamente.

Se non sono stati creati studenti in test, verrà visualizzata una pagina vuota sotto l'intestazione **statistiche corpo studente** . Andare alla pagina **studenti** , fare clic su **Aggiungi studente**e aggiungere alcuni studenti, quindi tornare alla pagina **About (informazioni** ) per visualizzare le statistiche degli studenti.

![Pagina informazioni nell'ambiente di test](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opzioni della riga di comando chiave

Il comando immesso passa il percorso del file di soluzione e due proprietà a MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Distribuzione della soluzione rispetto alla distribuzione di singoli progetti

Se si specifica il file di soluzione, verranno compilati tutti i progetti della soluzione. Se si dispone di più progetti Web nella soluzione, si applica il comportamento di MSBuild seguente:

- Le proprietà specificate nella riga di comando vengono passate a ogni progetto. Pertanto, ogni progetto Web deve disporre di un profilo di pubblicazione con il nome specificato. Se si specifica `/p:PublishProfile=Test`, ogni progetto Web deve disporre di un profilo di pubblicazione denominato *test*.
- È possibile pubblicare correttamente un progetto quando un altro non è ancora in fase di compilazione. Per ulteriori informazioni, vedere il thread StackOverflow [MSBuild ha esito negativo con due pacchetti](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Se si specifica un progetto singolo anziché una soluzione, è necessario aggiungere un parametro che specifichi la versione di Visual Studio. Se si usa Visual Studio 2012, la riga di comando sarà simile all'esempio seguente:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Il numero di versione per Visual Studio 2010 è 10,0. Per altre informazioni, vedere [compatibilità dei progetti di Visual Studio e VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) nel Blog di Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Specifica del profilo di pubblicazione

È possibile specificare il profilo di pubblicazione in base al nome o al percorso completo del file con *estensione pubxml* , come illustrato nell'esempio seguente:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Metodi di pubblicazione Web supportati per la pubblicazione da riga di comando

Per la pubblicazione da riga di comando sono supportati tre metodi di pubblicazione:

- `MSDeploy` pubblicare utilizzando Distribuzione Web.
- `Package`-pubblicare creando un pacchetto di Distribuzione Web. È necessario installare il pacchetto separatamente dal comando MSBuild che la crea.
- `FileSystem`-pubblicare copiando i file in una cartella specificata.

### <a name="specifying-the-build-configuration-and-platform"></a>Specifica della configurazione e della piattaforma di compilazione

La configurazione e la piattaforma di compilazione devono essere impostate in Visual Studio o dalla riga di comando. I profili di pubblicazione includono proprietà denominate `LastUsedBuildConfiguration` e `LastUsedPlatform`, ma non è possibile impostare queste proprietà per determinare la modalità di compilazione del progetto. Per altre informazioni, vedere [MSBuild: come impostare la proprietà di configurazione](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) nel Blog di Sayed Hashimi.

## <a name="deploy-to-staging"></a>Eseguire la distribuzione per lo staging

Per eseguire la distribuzione in Azure, è necessario aggiungere la password alla riga di comando. Se la password è stata salvata nel profilo di pubblicazione in Visual Studio, è stata archiviata in formato crittografato nel file con *estensione pubxml. User* . Il file non è accessibile da MSBuild quando si esegue una distribuzione da riga di comando, quindi è necessario passare la password in un parametro della riga di comando.

1. Copiare la password necessaria dal file con *estensione publishsettings* scaricato in precedenza per il sito Web di gestione temporanea. La password corrisponde al valore dell'attributo `userPWD` per l'elemento `publishProfile` Distribuzione Web.

    ![Password Distribuzione Web](command-line-deployment/_static/image5.png)
2. Nella pagina iniziale di Windows 8 cercare **prompt dei comandi per gli sviluppatori per VS2012**e fare clic sull'icona per aprire il prompt dei comandi. Non è necessario aprirlo come amministratore questa volta perché non si esegue la distribuzione in IIS nel computer locale.
3. Al prompt dei comandi immettere il comando seguente, sostituendo il percorso del file di soluzione con il percorso del file della soluzione e la password con la password:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Si noti che questa riga di comando include un parametro aggiuntivo: `/p:AllowUntrustedCertificate=true`. Durante la scrittura di questa esercitazione, è necessario impostare la proprietà `AllowUntrustedCertificate` quando si esegue la pubblicazione in Azure dalla riga di comando. Quando la correzione per questo bug viene rilasciata, questo parametro non è necessario.
4. Aprire un browser e passare all'URL del sito di gestione temporanea, quindi fare clic sulla pagina **informazioni su** per verificare che la distribuzione sia stata completata correttamente.

    Come illustrato in precedenza per l'ambiente di test, potrebbe essere necessario creare alcuni studenti per visualizzare le statistiche nella pagina **About (informazioni** ).

## <a name="deploy-to-production"></a>Distribuzione nell'ambiente di produzione

Il processo di distribuzione nell'ambiente di produzione è simile al processo di gestione temporanea.

1. Copiare la password necessaria dal file con *estensione publishsettings* scaricato in precedenza per il sito Web di produzione.
2. Aprire **prompt dei comandi per gli sviluppatori per VS2012**.
3. Al prompt dei comandi immettere il comando seguente, sostituendo il percorso del file di soluzione con il percorso del file della soluzione e la password con la password:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Per un sito di produzione reale, se si è verificata una modifica del database, in genere si copia l' *app\_file offline. htm* nel sito prima della distribuzione ed eliminarla dopo la corretta distribuzione.
4. Aprire un browser e passare all'URL del sito di gestione temporanea, quindi fare clic sulla pagina **informazioni su** per verificare che la distribuzione sia stata completata correttamente.

## <a name="summary"></a>Riepilogo

A questo punto è stato distribuito un aggiornamento dell'applicazione tramite la riga di comando.

![Pagina informazioni nell'ambiente di test](command-line-deployment/_static/image6.png)

Nell'esercitazione successiva verrà visualizzato un esempio di come estendere la pipeline di pubblicazione Web. Nell'esempio viene illustrato come distribuire i file che non sono inclusi nel progetto.

> [!div class="step-by-step"]
> [Precedente](deploying-a-database-update.md)
> [Successivo](deploying-extra-files.md)
