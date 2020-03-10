---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Distribuzione Web ASP.NET con Visual Studio: Proprietà progetto | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: b2811791a897c9166f6222c23dddc6921e5267ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78643598"
---
# <a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Distribuzione Web ASP.NET con Visual Studio: proprietà del progetto

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica

Alcune opzioni di distribuzione vengono configurate nelle proprietà del progetto archiviate nel file di progetto (file con *estensione csproj* o *VBPROJ* ). Nella maggior parte dei casi, i valori predefiniti di queste impostazioni sono quelli desiderati, ma è possibile usare l'interfaccia utente delle **proprietà del progetto** incorporata in Visual Studio per usare queste impostazioni se è necessario modificarle. In questa esercitazione vengono esaminate le impostazioni di distribuzione nelle **proprietà del progetto**. Si crea anche un file segnaposto che causa la distribuzione di una cartella vuota.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Configurare le impostazioni di distribuzione nella finestra delle proprietà del progetto

La maggior parte delle impostazioni che interessano cosa accade durante la distribuzione è inclusa nel profilo di pubblicazione, come si vedrà nelle esercitazioni seguenti. Alcune impostazioni che è necessario tenere presenti si trovano nelle schede **pubblicazione/pacchetto** della finestra **Proprietà progetto** . Queste impostazioni vengono specificate per ogni configurazione della build, ovvero è possibile avere impostazioni diverse per una build di rilascio rispetto a una build di debug.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **ContosoUniversity** , scegliere **Proprietà**, quindi selezionare la scheda **pacchetto/pubblica sito Web** .

![Scheda Pubblicazione/creazione pacchetto Web](project-properties/_static/image1.png)

Quando la finestra viene visualizzata, per impostazione predefinita vengono visualizzate le impostazioni per qualsiasi configurazione di compilazione attualmente attiva per la soluzione. Se la casella di **configurazione** non indica **Active (Release)** , selezionare **Release** per visualizzare le impostazioni per la configurazione della build di rilascio. Verranno distribuite le build di rilascio negli ambienti di test e di produzione.

![Selezione della configurazione della build di rilascio](project-properties/_static/image2.png)

Con la versione **attiva (rilascio)** o la **versione** selezionata, vengono visualizzati i valori effettivi quando si esegue la distribuzione usando la configurazione della build di rilascio:

- Nella casella **elementi da distribuire** sono selezionati **solo i file necessari per eseguire l'applicazione** . Altre opzioni sono **tutti i file in questo progetto** o **tutti i file in questa cartella del progetto**. Lasciando invariata la selezione predefinita, è possibile evitare di distribuire i file del codice sorgente, ad esempio. Questa impostazione è il motivo per cui le cartelle che contengono i file binari di SQL Server Compact devono essere incluse nel progetto. Per altre informazioni su questa impostazione, vedere **perché non tutti i file nella cartella del progetto vengono distribuiti?** in [domande frequenti sulla distribuzione del progetto di applicazione Web ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx).
- **Escludi simboli di debug generati** è selezionata. Non verrà eseguito il debug quando si usa questa configurazione di compilazione.
- È selezionata l'opzione **Includi tutti i database configurati nella scheda pubblicazione/pacchetto SQL** . Specifica se in Visual Studio vengono distribuiti i database e i file. Sebbene l'etichetta della casella di controllo menzioni solo la scheda **pubblicazione/pubblicazione pacchetto SQL** , deselezionare questa casella di controllo per disabilitare anche la distribuzione del database configurata nel profilo di pubblicazione. Questa operazione verrà apportata in un secondo momento, quindi la casella di controllo deve rimanere selezionata. La scheda **pacchetto/pubblica SQL** viene usata per un metodo di pubblicazione del database legacy che non verrà usato in queste esercitazioni.
- La sezione **delle impostazioni del pacchetto di distribuzione Web** non è applicabile perché si sta usando la pubblicazione con un clic in queste esercitazioni.

Modificare la casella di riepilogo a discesa **configurazione** in debug per visualizzare le impostazioni predefinite per le compilazioni di debug. I valori sono uguali, ad eccezione del fatto che **Escludi i simboli di debug generati** vengono cancellati in modo da poter eseguire il debug quando si distribuisce una compilazione di debug.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Assicurarsi che la cartella ELMAH venga distribuita

Come illustrato nell'esercitazione precedente, il [pacchetto NuGet ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornisce funzionalità per la registrazione e la creazione di report degli errori. Nell'applicazione Contoso University ELMAH è stato configurato per archiviare i dettagli degli errori in una cartella denominata *ELMAH*:

![Cartella ELMAH](project-properties/_static/image3.png)

L'esclusione di specifici file o cartelle dalla distribuzione è un requisito comune. un altro esempio è costituito da una cartella in cui gli utenti possono caricare file. Non si vuole che i file di log o i file caricati creati nell'ambiente di sviluppo siano distribuiti nell'ambiente di produzione. Se si distribuisce un aggiornamento in un ambiente di produzione, non si vuole che il processo di distribuzione elimini i file presenti nell'ambiente di produzione. A seconda della modalità di impostazione di un'opzione di distribuzione, se un file esiste nel sito di destinazione ma non nel sito di origine quando si distribuisce, Distribuzione Web lo elimina dalla destinazione.

Come illustrato in precedenza in questa esercitazione, l'opzione **elementi da distribuire** nella scheda **pubblicazione/pubblicazione pacchetto Web** è impostata su **solo i file necessari per eseguire l'applicazione**. Di conseguenza, i file di log creati da ELMAH in fase di sviluppo non verranno distribuiti, ovvero ciò che si desidera eseguire. (Per essere distribuite, devono essere incluse nel progetto e la relativa proprietà dell' **azione di compilazione** deve essere impostata sul **contenuto**. Per altre informazioni, vedere **perché non tutti i file nella cartella del progetto vengono distribuiti?** in [domande frequenti sulla distribuzione del progetto di applicazione Web ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx)). Tuttavia, Distribuzione Web non creerà una cartella nel sito di destinazione a meno che non vi sia almeno un file in cui eseguire la copia. Pertanto, si aggiungerà un file con *estensione txt* alla cartella per fungere da segnaposto, in modo che la cartella venga copiata.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *ELMAH* , scegliere **Aggiungi nuovo elemento**e creare un file di testo denominato *segnaposto. txt*. Inserire il testo seguente: "si tratta di un file segnaposto per assicurarsi che la cartella venga distribuita". e salvare il file. Questo è tutto ciò che è necessario fare per assicurarsi che Visual Studio distribuisca questo file e la cartella in cui si trova, perché la proprietà **azione di compilazione** dei file con *estensione txt* è impostata su **contenuto** per impostazione predefinita.

## <a name="summary"></a>Riepilogo

A questo punto sono state completate tutte le attività di configurazione della distribuzione. Nell'esercitazione successiva il sito Contoso University verrà distribuito nell'ambiente di test e testato.

> [!div class="step-by-step"]
> [Precedente](web-config-transformations.md)
> [Successivo](deploying-to-iis.md)
