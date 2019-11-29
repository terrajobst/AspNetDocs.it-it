---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: configurazione delle proprietà del progetto-4 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6e63e75dca3d776fb9a1bd7e420ef48891daac69
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569804"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact mediante Visual Studio o Visual Web Developer: configurazione delle proprietà del progetto-4 di 12

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un progetto di applicazione Web ASP.NET che include un database di SQL Server Compact usando Visual Studio 2012 RC o Visual Studio Express 2012 RC per il Web. È anche possibile usare Visual Studio 2010 se si installa l'aggiornamento pubblicazione sul Web. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione in cui vengono illustrate le funzionalità di distribuzione introdotte dopo la versione RC di Visual Studio 2012, viene illustrato come distribuire SQL Server edizioni diverse da SQL Server Compact e viene illustrato come eseguire la distribuzione in app Azure app Web del servizio, vedere [distribuzione web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Panoramica di

Alcune opzioni di distribuzione vengono configurate nelle proprietà del progetto archiviate nel file di progetto (file con *estensione csproj* o *VBPROJ* ). Nella maggior parte dei casi, i valori predefiniti di queste impostazioni sono quelli desiderati, ma è possibile usare l'interfaccia utente delle **proprietà del progetto** incorporata in Visual Studio per usare queste impostazioni se è necessario modificarle. In questa esercitazione vengono esaminate le impostazioni di distribuzione nelle **proprietà del progetto**. Si crea anche un file segnaposto che causa la distribuzione di una cartella vuota.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Configurazione delle impostazioni di distribuzione nella finestra delle proprietà del progetto

La maggior parte delle impostazioni che interessano cosa accade durante la distribuzione è inclusa nel profilo di pubblicazione, come si vedrà nelle esercitazioni seguenti. Alcune impostazioni che è necessario tenere presenti si trovano nelle schede **pubblicazione/pacchetto** della finestra **Proprietà progetto** . Queste impostazioni vengono specificate per ogni configurazione della build, ovvero è possibile avere impostazioni diverse per una build di rilascio rispetto a una build di debug.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **ContosoUniversity** , scegliere **Proprietà**, quindi selezionare la scheda **pacchetto/pubblica sito Web** .

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Quando la finestra viene visualizzata, per impostazione predefinita vengono visualizzate le impostazioni per qualsiasi configurazione di compilazione attualmente attiva per la soluzione. Se la casella di **configurazione** non indica **Active (Release)** , selezionare **Release** per visualizzare le impostazioni per la configurazione della build di rilascio. Verranno distribuite le build di rilascio negli ambienti di test e di produzione.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Con la versione **attiva (rilascio)** o la **versione** selezionata, vengono visualizzati i valori effettivi quando si esegue la distribuzione usando la configurazione della build di rilascio:

- Nella casella **elementi da distribuire** sono selezionati **solo i file necessari per eseguire l'applicazione** . Altre opzioni sono **tutti i file in questo progetto** o **tutti i file in questa cartella del progetto**. Lasciando invariata la selezione predefinita, è possibile evitare di distribuire i file del codice sorgente, ad esempio. Questa impostazione è il motivo per cui le cartelle che contengono i file binari di SQL Server Compact devono essere incluse nel progetto. Per altre informazioni su questa impostazione, vedere **perché non tutti i file nella cartella del progetto vengono distribuiti?** in [domande frequenti sulla distribuzione del progetto di applicazione Web ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx).
- **Escludi simboli di debug generati** è selezionata. Non verrà eseguito il debug quando si usa questa configurazione di compilazione.
- Non è stata selezionata **l'opzione Escludi file dalla cartella App\_data** . Il file di SQL Server Compact per il database delle appartenenze si trova in tale cartella ed è necessario distribuirla. Quando si distribuiscono aggiornamenti che non includono modifiche al database, selezionare questa casella di controllo.
- **Precompilare questa applicazione prima che la pubblicazione** non sia selezionata. Nella maggior parte degli scenari non è necessario precompilare i progetti di applicazioni Web. Per altre informazioni su questa opzione, vedere la finestra di dialogo [pacchetto/pubblica Web, le proprietà del progetto](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) e [le impostazioni avanzate di precompilazione](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- È selezionata l'opzione **Includi tutti i database configurati nella scheda pubblicazione/pacchetto SQL** , ma questa opzione non ha effetto adesso perché non si configura la scheda **pubblicazione/pubblicazione pacchetto SQL** . Questa scheda è destinata a un metodo di distribuzione di database legacy che era l'unica opzione per la distribuzione di SQL Server database. Si userà la scheda **pacchetto/pubblica SQL** nell'esercitazione [migrazione a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) .
- La sezione **delle impostazioni del pacchetto di distribuzione Web** non è applicabile perché si sta usando la pubblicazione con un clic in queste esercitazioni.

Modificare la casella di riepilogo a discesa **configurazione** in debug per visualizzare le impostazioni predefinite per le compilazioni di debug. I valori sono uguali, ad eccezione del fatto che **Escludi i simboli di debug generati** vengono cancellati in modo da poter eseguire il debug quando si distribuisce una compilazione di debug.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Assicurarsi che la cartella ELMAH venga distribuita

Come illustrato nell'esercitazione precedente, il [pacchetto NuGet ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornisce funzionalità per la registrazione e la creazione di report degli errori. Nell'applicazione Contoso University ELMAH è stato configurato per archiviare i dettagli degli errori in una cartella denominata *ELMAH*:

![Cartella ELMAH](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

L'esclusione di specifici file o cartelle dalla distribuzione è un requisito comune. un altro esempio è costituito da una cartella in cui gli utenti possono caricare file. Non si vuole che i file di log o i file caricati creati nell'ambiente di sviluppo siano distribuiti nell'ambiente di produzione. Se si distribuisce un aggiornamento in un ambiente di produzione, non si vuole che il processo di distribuzione elimini i file presenti nell'ambiente di produzione. A seconda della modalità di impostazione di un'opzione di distribuzione, se un file esiste nel sito di destinazione ma non nel sito di origine quando si distribuisce, Distribuzione Web lo elimina dalla destinazione.

Come illustrato in precedenza in questa esercitazione, l'opzione **elementi da distribuire** nella scheda **pubblicazione/pubblicazione pacchetto Web** è impostata su **solo i file necessari per eseguire l'applicazione**. Di conseguenza, i file di log creati da ELMAH in fase di sviluppo non verranno distribuiti, ovvero ciò che si desidera eseguire. (Per essere distribuite, devono essere incluse nel progetto e la relativa proprietà dell' **azione di compilazione** deve essere impostata sul **contenuto**. Per altre informazioni, vedere **perché non tutti i file nella cartella del progetto vengono distribuiti?** in [domande frequenti sulla distribuzione del progetto di applicazione Web ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx)). Tuttavia, Distribuzione Web non creerà una cartella nel sito di destinazione a meno che non vi sia almeno un file in cui eseguire la copia. Pertanto, si aggiungerà un file con *estensione txt* alla cartella per fungere da segnaposto, in modo che la cartella venga copiata.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *ELMAH* , scegliere **Aggiungi nuovo elemento**e creare un file denominato *segnaposto. txt*. Inserire il testo seguente: "si tratta di un file segnaposto per assicurarsi che la cartella venga distribuita". e salvare il file. Questo è tutto ciò che è necessario fare per assicurarsi che Visual Studio distribuisca questo file e la cartella in cui si trova, perché la proprietà **azione di compilazione** dei file con *estensione txt* è impostata su **contenuto** per impostazione predefinita.

A questo punto sono state completate tutte le attività di configurazione della distribuzione. Nell'esercitazione successiva il sito Contoso University verrà distribuito nell'ambiente di test e testato.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
