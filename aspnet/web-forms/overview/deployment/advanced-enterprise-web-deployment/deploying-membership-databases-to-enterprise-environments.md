---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Distribuzione dei database di appartenenza in ambienti aziendali | Microsoft Docs
author: jrjlee
description: In questo argomento vengono illustrate le principali considerazioni e problemi che è necessario affrontare quando si effettua il provisioning dei database dei servizi delle applicazioni ASP.NET (più comuni...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 50f49af502b75aa5ad52756a76a5e7340aca53f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526005"
---
# <a name="deploying-membership-databases-to-enterprise-environments"></a>Distribuzione dei database di appartenenza negli ambienti aziendali

di [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento vengono illustrate le principali considerazioni e problemi che è necessario affrontare quando si effettua il provisioning di database dei servizi delle applicazioni ASP.NET (più comunemente definiti database di appartenenza) negli ambienti di test, di gestione temporanea o di produzione. Vengono inoltre descritti gli approcci che è possibile utilizzare per rispondere a tali problemi.

Questo argomento fa parte di una serie di esercitazioni basate sui requisiti di distribuzione aziendali di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni usa una&#x2014;soluzione di esempio la&#x2014; [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)per rappresentare un'applicazione Web con un livello di complessità realistico, tra cui un'applicazione ASP.NET MVC 3, un servizio Windows Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione al centro di queste esercitazioni si basa sull'approccio Split Project file descritto in [informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due&#x2014;file di progetto, uno contenente istruzioni di compilazione che si applicano a ogni ambiente di destinazione e uno contenente le impostazioni di compilazione e distribuzione specifiche dell'ambiente. In fase di compilazione, il file di progetto specifico dell'ambiente viene unito al file di progetto indipendente dall'ambiente per formare un set completo di istruzioni di compilazione.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Quali sono i problemi quando si distribuisce un database di appartenenza?

Nella maggior parte dei casi, quando si progetta una strategia di distribuzione per un database, è prima di tutto necessario considerare i dati che si desidera distribuire. In un ambiente di sviluppo o di test, è possibile distribuire i dati dell'account utente per facilitare i test rapidi e semplici. In un ambiente di gestione temporanea o di produzione, è molto improbabile che si desideri distribuire i dati dell'account utente.

Sfortunatamente, i database di appartenenza a ASP.NET introducono alcune difficoltà specifiche che rendono questa decisione molto più complessa:

- Una distribuzione di solo schema lascerà il database di appartenenza in uno stato non operativo. Questo è dovuto al fatto che il database di appartenenza include alcuni dati di configurazione (nella tabella **aspnet\_SchemaVersions** ) necessari per il funzionamento del database. Di conseguenza, se si esegue una distribuzione solo schema del database di appartenenza per escludere i dati dell'account utente, è necessario eseguire uno script di post-distribuzione per aggiungere i dati di configurazione essenziali.
- A seconda di come viene configurato il database di appartenenza, il provider di appartenenze può utilizzare la chiave del computer per crittografare le password e archiviarle nel database. In questo caso, tutti i dati dell'account utente distribuiti con il database diventeranno inutilizzabili nel server di destinazione. Per questo motivo, la distribuzione dei dati dell'account utente non è uno scenario supportato.

## <a name="choosing-a-membership-database-strategy"></a>Scelta di una strategia di database di appartenenza

Utilizzare queste linee guida quando si sceglie come effettuare il provisioning di un database di appartenenza in un ambiente Enterprise Server:

- Laddove possibile, non distribuire i database di appartenenza. Al contrario, creare manualmente il database delle appartenenze nel server di database di destinazione. Se lo schema del database di appartenenza non è stato personalizzato, è sufficiente crearne uno nuovo in situ nella destinazione usando lo [strumento di registrazione ASP.NET SQL Server (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Se non si dispone di un'opzione ma si distribuisce&#x2014;un database di appartenenza, ad esempio se sono state apportate&#x2014;modifiche estese allo schema del database, è consigliabile eseguire una distribuzione solo schema del database di appartenenza, per escludere i dati dell'account utente, quindi eseguire uno script di post-distribuzione per aggiungere i dati di configurazione necessari. Per informazioni generali su questi approcci [, vedere Procedura: distribuire il database delle appartenenze di ASP.NET senza includere gli account utente](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

È importante ricordare che *lo schema del database di appartenenza è probabilmente piuttosto statico*. Anche se il database di appartenenza è stato personalizzato, è improbabile che sia necessario aggiornare lo schema a intervalli regolari,&#x2014;ma non cambierà con la stessa frequenza del codice in un'applicazione Web o in un progetto di database. Di conseguenza, non è necessario includere il database di appartenenza in tutti i processi di distribuzione automatizzati o in un singolo passaggio.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Utilizzo di VSDBCMD per aggiornare uno schema del database di appartenenza

Se si modifica la struttura del database di appartenenza dopo la prima distribuzione, è possibile che non si desideri utilizzare lo strumento di distribuzione Web (Distribuzione Web) Internet Information Services (IIS) per ridistribuire il database. La funzionalità di distribuzione del database in Distribuzione Web non include la possibilità di eseguire aggiornamenti differenziali in&#x2014;un database di destinazione, distribuzione Web necessario eliminare e ricreare il database. Ciò significa che si perdono tutti i dati esistenti dell'account utente, che in genere non sono desiderati negli ambienti di gestione temporanea o di produzione.

In alternativa, è possibile utilizzare l'utilità VSDBCMD per aggiornare lo schema del database di destinazione. VSDBCMD include due funzionalità importanti. In primo luogo, è possibile importare lo schema di un database esistente in un file con estensione dbschema. In secondo luogo, è in grado di distribuire un file con estensione dbschema a un database esistente come aggiornamento differenziale, il che significa che apporta solo le modifiche necessarie per portare aggiornato il database di destinazione e non si perdono i dati.

È possibile utilizzare questi passaggi di alto livello per aggiornare uno schema del database di appartenenza:

1. Usare l'azione di **importazione** VSDBCMD per generare un file con estensione dbschema per il database delle appartenenze di origine. Questa procedura è descritta in [procedura: importare uno schema da un prompt dei comandi](https://msdn.microsoft.com/library/dd172135.aspx).
2. Usare l'azione di **distribuzione** VSDBCMD per distribuire il file con estensione dbschema nel database delle appartenenze di destinazione. Questa procedura è descritta in [riferimento alla riga di comando per VSDBCMD. EXE (distribuzione e importazione dello schema)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusione

Questo argomento descrive alcune delle problemi che è possibile affrontare quando è necessario eseguire il provisioning di database di appartenenza a ASP.NET in diversi ambienti di destinazione. In particolare, spiega perché le distribuzioni solo schema lasceranno il database di appartenenza in uno stato non operativo e perché la distribuzione dei dati dell'account utente non è supportata. Nell'argomento vengono inoltre fornite indicazioni su come eseguire il provisioning, distribuire e aggiornare i database di appartenenza in scenari diversi.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre indicazioni ed esempi sull'uso di VSDBCMD, vedere informazioni di [riferimento sulla riga di comando per VSDBCMD. EXE (distribuzione e importazione dello schema)](https://msdn.microsoft.com/library/dd193283.aspx) e [procedura: importare uno schema da un prompt dei comandi](https://msdn.microsoft.com/library/dd172135.aspx). Per ulteriori informazioni sull'utilizzo di ASPNET\_RegSql. exe per creare database di appartenenza, vedere [ASP.NET SQL Server Registration Tool (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Per istruzioni più generali sulla distribuzione dei database di appartenenza, vedere [procedura: distribuire il database delle appartenenze ASP.NET senza includere gli account utente](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Precedente](deploying-database-role-memberships-to-test-environments.md)
> [Successivo](excluding-files-and-folders-from-deployment.md)
