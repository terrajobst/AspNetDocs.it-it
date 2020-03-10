---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET ha negato l'accesso alle directory IIS | Microsoft Docs
author: rick-anderson
description: In questo white paper vengono descritte le operazioni che è necessario eseguire se una richiesta all'applicazione ASP.NET restituisce l'errore "accesso negato alla directory DirectoryName. Non è stato possibile...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638502"
---
# <a name="aspnet-denied-access-to-iis-directories"></a>Accesso negato alle directory di IIS per ASP.NET

> In questo white paper vengono descritte le operazioni che è necessario eseguire se una richiesta all'applicazione ASP.NET restituisce l'errore "accesso negato alla directory *DirectoryName* . Non è stato possibile avviare il monitoraggio delle modifiche della directory. "
> 
> Si applica a ASP.NET 1,0 e ASP.NET 1,1.

ASP.NET V1 RTM viene ora eseguito con un account Windows meno privilegiato, registrato come account "ASPNET" in un computer locale.

In alcuni sistemi bloccati, per impostazione predefinita, questo account non dispone dell'accesso in lettura per la sicurezza alle directory di contenuto di un sito Web, alla directory radice dell'applicazione o alla directory radice del sito Web. In questo caso verrà visualizzato l'errore seguente durante la richiesta di pagine da una determinata applicazione Web:

![](denied-access-to-iis-directories/_static/image1.jpg)

Per risolvere questo problema, sarà necessario modificare le autorizzazioni di sicurezza nelle directory appropriate.

In particolare, ASP.NET richiede l'accesso in lettura, esecuzione ed elenco per l'account ASPNET per la radice del sito Web (ad esempio: c:\Inetpub\Wwwroot o qualsiasi directory del sito alternativa configurata in IIS), la directory del contenuto e la directory radice dell'applicazione per monitorare le modifiche del file di configurazione. La radice dell'applicazione corrisponde al percorso della cartella associato alla directory virtuale dell'applicazione nello strumento di amministrazione di IIS (inetmgr).

Si consideri, ad esempio, la seguente gerarchia dell'applicazione nella cartella wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

Per questo esempio, l'account ASPNET richiede le autorizzazioni di lettura definite in precedenza per il contenuto nella directory MyApp e wwwroot. Un singolo ACL ereditato nella cartella radice può anche essere usato facoltativamente per entrambe le directory se sono annidate.

Per aggiungere le autorizzazioni a una directory, seguire questa procedura:

- Utilizzando Esplora risorse, passare alla directory
- Fare clic con il pulsante destro del mouse sulla cartella directory e scegliere "proprietà"
- Passare alla scheda "sicurezza" nella finestra di dialogo delle proprietà
- Fare clic sul pulsante "Aggiungi" e immettere il nome del computer seguito dal nome dell'account ASPNET. Ad esempio, in un computer denominato "WebDev", immettere webdev\ASPNET e fare clic su "OK".
- Verificare che l'account ASPNET includa le caselle di controllo "Read &amp; Execute", "List Folder Contents" e "Read".
- Fare clic su OK per chiudere la finestra di dialogo e salvare le modifiche.

![](denied-access-to-iis-directories/_static/image2.jpg)

Se necessario, queste modifiche possono essere automatizzate tramite script o lo strumento "cacls. exe" fornito con Windows. Per ulteriori informazioni sull'account ASPNET, vedere il documento di [domande frequenti](https://go.microsoft.com/fwlink/?LinkId=5828).

Se una determinata applicazione Web si basa sulla presenza di autorizzazioni di scrittura o modifica per una cartella o un file specifico, è possibile concederlo seguendo la stessa procedura e selezionando le caselle di controllo "Write" e/o "Modify".

Nei computer che consentono a tutti o al gruppo di utenti di accedere in lettura a tali directory (ovvero la configurazione predefinita), non verrà rilevato alcun problema e i passaggi precedenti non saranno necessari.
