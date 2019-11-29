---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Distribuzione Web ASP.NET con Visual Studio: distribuzione di file aggiuntivi | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: eaa3141c22980f0c816e2f33b5597ac9fe69c23c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594898"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Distribuzione Web ASP.NET con Visual Studio: distribuzione di file aggiuntivi

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica di

Questa esercitazione illustra come estendere la pipeline di pubblicazione Web di Visual Studio per eseguire un'attività aggiuntiva durante la distribuzione. L'attività consiste nel copiare i file aggiuntivi che non sono presenti nella cartella del progetto nel sito Web di destinazione.

Per questa esercitazione verrà copiato un file aggiuntivo: *robots. txt*. Si vuole distribuire questo file nella gestione temporanea ma non nell'ambiente di produzione. Nell'esercitazione [distribuzione in produzione](deploying-to-production.md) questo file è stato aggiunto al progetto e configurato per escluderlo dal profilo di pubblicazione di produzione. In questa esercitazione verrà visualizzato un metodo alternativo per gestire questa situazione, una che sarà utile per tutti i file che si desidera distribuire, ma che non si desidera includere nel progetto.

## <a name="move-the-robotstxt-file"></a>Spostare il file robots. txt

Per preparare un metodo diverso per la gestione di *robots. txt*, in questa sezione dell'esercitazione si sposta il file in una cartella che non è inclusa nel progetto e si elimina *robots. txt* dall'ambiente di gestione temporanea. È necessario eliminare il file dalla gestione temporanea per poter verificare che il nuovo metodo di distribuzione del file nell'ambiente funzioni correttamente.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul file *robots. txt* e scegliere **Escludi dal progetto**.
2. Utilizzando Esplora file di Windows, creare una nuova cartella nella cartella della soluzione e denominarla *file*.
3. Spostare il file *robots. txt* dalla cartella del progetto *ContosoUniversity* alla cartella *file* con estensione.

    ![Cartella file con estensione](deploying-extra-files/_static/image1.png)
4. Utilizzando lo strumento FTP, eliminare il file *robots. txt* dal sito Web di gestione temporanea.

    In alternativa, è possibile selezionare **Rimuovi file aggiuntivi nella destinazione** in **Opzioni di pubblicazione file** nella scheda **Impostazioni** del profilo di pubblicazione di staging e ripubblicare in gestione temporanea.

## <a name="update-the-publish-profile-file"></a>Aggiornare il file del profilo di pubblicazione

È necessario solo *robots. txt* in staging, quindi l'unico profilo di pubblicazione che è necessario aggiornare per distribuirlo è staging.

1. In Visual Studio aprire *staging. pubxml*.
2. Alla fine del file, prima del tag di chiusura `</Project>` aggiungere il markup seguente:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Questo codice crea una nuova *destinazione* in cui verranno raccolti i file aggiuntivi da distribuire. Una destinazione è costituita da una o più attività che MSBuild verrà eseguita in base alle condizioni specificate.

    L'attributo `Include` specifica che la cartella in cui trovare i file è costituita da *file*, che si trovano allo stesso livello della cartella del progetto. MSBuild raccoglierà tutti i file dalla cartella e in modo ricorsivo da qualsiasi sottocartella (il doppio asterisco specifica le sottocartelle ricorsive). Con questo codice è possibile inserire più file e file nelle sottocartelle all'interno della cartella di *file* con estensione outfiles e tutti verranno distribuiti.

    L'elemento `DestinationRelativePath` specifica che le cartelle e i file devono essere copiati nella cartella radice del sito Web di destinazione, nella stessa struttura di file e cartelle in cui si trovano nella cartella *file* . Se si desidera copiare la cartella dei file con estensione di *file* , il valore di `DestinationRelativePath` sarà *file\%(RecursiveDir)% (filename)% (Extension)* .
3. Alla fine del file, prima del tag di chiusura `</Project>` aggiungere il markup seguente che specifica quando eseguire la nuova destinazione.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Questo codice fa in modo che la nuova destinazione `CustomCollectFiles` venga eseguita ogni volta che viene eseguita la destinazione che copia i file nella cartella di destinazione. Esiste una destinazione separata per la creazione del pacchetto di pubblicazione e distribuzione e la nuova destinazione viene inserita in entrambe le destinazioni nel caso in cui si decida di eseguire la distribuzione usando un pacchetto di distribuzione anziché la pubblicazione.

    Il file con *estensione pubxml* è ora simile all'esempio seguente:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Salvare e chiudere il file *staging. pubxml* .

## <a name="publish-to-staging"></a>Pubblica in gestione temporanea

Utilizzando la pubblicazione con un clic o la riga di comando, pubblicare l'applicazione tramite il profilo di gestione temporanea.

Se si usa la pubblicazione con un clic, è possibile verificare nella finestra di **Anteprima** che verrà copiato *robots. txt* . In caso contrario, usare lo strumento FTP per verificare che il file *robots. txt* si trovi nella cartella radice del sito Web dopo la distribuzione.

## <a name="summary"></a>Riepilogo

Questa operazione completa questa serie di esercitazioni sulla distribuzione di un'applicazione Web ASP.NET a un provider di hosting di terze parti. Per ulteriori informazioni sugli argomenti trattati in queste esercitazioni, vedere la mappa del [contenuto della distribuzione di ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Altre informazioni

Se si sa come usare i file di MSBuild, è possibile automatizzare molte altre attività di distribuzione scrivendo il codice nei file *. pubxml* (per le attività specifiche del profilo) o il file Project *. WPP. targets* (per le attività che si applicano a tutti i profili). Per altre informazioni sui file con estensione *pubxml* e *WPP. targets* , vedere [procedura: modificare le impostazioni di distribuzione nei file del profilo di pubblicazione (con estensione pubxml) e il file con estensione WPP. targets nei progetti Web di Visual Studio](https://msdn.microsoft.com/library/ff398069). Per un'introduzione di base al codice MSBuild, vedere l'articolo relativo **all'anatomia di un file di progetto** in [serie Enterprise Deployment: informazioni sul file di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Per informazioni su come usare i file di MSBuild per eseguire attività per i propri scenari, vedere questo libro: [all'interno del Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://msbuildbook.com) di Sayed Ibraham Hashimi e William Bartholomew.

## <a name="acknowledgements"></a>Riconoscimenti

Desidero ringraziare i seguenti utenti che hanno apportato contributi significativi al contenuto di questa serie di esercitazioni:

- [Alberto Poblacion, MVP &amp; MCT, Spagna](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, MVP per lo sviluppo di piattaforme dati, Stati Uniti
- Duro Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Sauro Srivastava, Microsoft
- [Raffaele Rialdi, Italia](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott hanseln](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- Alessandro [Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Precedente](command-line-deployment.md)
> [Successivo](troubleshooting.md)
