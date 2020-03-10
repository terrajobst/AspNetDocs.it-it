---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Creare un nuovo progetto MVC ASP.NET | Microsoft Docs
author: microsoft
description: Il passaggio 1 Mostra come posizionare la struttura dell'applicazione NerdDinner di base.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 189ddc187fc83db14106b2da199ba12a70a32b45
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580934"
---
# <a name="create-a-new-aspnet-mvc-project"></a>Creare un nuovo progetto ASP.NET MVC

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 1 di un' [applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuita che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.
> 
> Il passaggio 1 Mostra come posizionare la struttura dell'applicazione NerdDinner di base.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner passaggio 1: file-&gt;nuovo progetto

Si inizierà l'applicazione NerdDinner selezionando la voce di menu **nuovo progetto&gt;file** all'interno di visual Studio 2008 o la versione gratuita di Visual Web Developer 2008 Express.

Verrà visualizzata la finestra di dialogo "nuovo progetto". Per creare una nuova applicazione MVC ASP.NET, selezionare il nodo "Web" sul lato sinistro della finestra di dialogo e quindi scegliere il modello di progetto "applicazione Web MVC ASP.NET" a destra:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: assicurarsi di aver scaricato e installato ASP.NET MVC. in caso contrario, non verrà visualizzato nella finestra di dialogo nuovo progetto. È possibile usare la versione V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) se non è ancora stata installata (ASP.NET MVC è disponibile all'interno della sezione "Web Platform-&gt;Frameworks and Runtimes").*

Il nuovo progetto verrà creato con il nome "NerdDinner" e quindi fare clic sul pulsante "OK" per crearlo.

Quando si fa clic su "OK", Visual Studio Visualizza una finestra di dialogo aggiuntiva che richiede di creare facoltativamente un progetto di unit test anche per la nuova applicazione. Questo progetto unit test consente di creare test automatizzati che verificano le funzionalità e il comportamento dell'applicazione (cosa verrà illustrato più avanti in questa esercitazione).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

L'elenco a discesa "Framework di test" nella finestra di dialogo precedente viene popolato con tutti i modelli di progetto unit test MVC ASP.NET installati nel computer. È possibile scaricare le versioni per NUnit, MBUnit e XUnit. È supportato anche il Framework di unit test predefinito di Visual Studio.

*Nota: il Framework per unit test di Visual Studio è disponibile solo con Visual Studio 2008 Professional e versioni successive. Se si utilizza VS 2008 Standard Edition o Visual Web Developer 2008 Express, sarà necessario scaricare e installare NUnit, MBUnit o XUnit Extensions for ASP.NET MVC per visualizzare la finestra di dialogo. Se non è installato alcun framework di test, la finestra di dialogo non viene visualizzata.*

Verrà usato il nome predefinito "NerdDinner. tests" per il progetto di test creato e verrà usata l'opzione del Framework "unit test di Visual Studio". Quando si fa clic sul pulsante "OK", Visual Studio creerà una soluzione per Microsoft con due progetti, uno per l'applicazione Web e uno per gli unit test:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Esame della struttura di directory NerdDinner

Quando si crea una nuova applicazione MVC ASP.NET con Visual Studio, vengono aggiunti automaticamente un numero di file e directory al progetto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Per impostazione predefinita, i progetti MVC ASP.NET hanno sei directory di primo livello:

| **Directory** | **Scopo** |
| --- | --- |
| **/Controllers** | Dove si inseriscono le classi controller che gestiscono le richieste URL |
| **/Models** | Posizione in cui inserire le classi che rappresentano e modificano i dati |
| **/Views** | Posizione in cui inserire i file di modello dell'interfaccia utente responsabili del rendering dell'output |
| **/Scripts** | Dove si inseriscono script e file di libreria JavaScript (. js) |
| **/Content** | Posizione in cui inserire i file di immagine e CSS e altri contenuti non dinamici/non JavaScript |
| **/App\_dati** | Dove vengono archiviati i file di dati che si desidera leggere/scrivere. |

ASP.NET MVC non richiede questa struttura. Infatti, gli sviluppatori che lavorano su applicazioni di grandi dimensioni in genere partizionano l'applicazione in più progetti per renderla più gestibile (ad esempio, le classi del modello di dati vengono spesso inserite in un progetto di libreria di classi separato dall'applicazione Web). La struttura di progetto predefinita, tuttavia, fornisce una valida convenzione di directory predefinita che è possibile utilizzare per garantire la pulizia dell'applicazione.

Quando si espande la directory/Controllers si noterà che Visual Studio ha aggiunto due classi controller, HomeController e AccountController, per impostazione predefinita al progetto:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Quando si espande la directory/views, sono state aggiunte al progetto anche tre sottodirectory, ovvero/Home,/account e/documenti condivisi, oltre a diversi file di modello al suo interno, per impostazione predefinita:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Quando si espandono le directory/content e/scripts, si troverà un file site. CSS usato per applicare lo stile a tutto il codice HTML nel sito, nonché le librerie JavaScript che possono abilitare il supporto per ASP.NET AJAX e jQuery all'interno dell'applicazione:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Quando si espande il progetto NerdDinner. tests, sono disponibili due classi che contengono unit test per le classi controller:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Questi file predefiniti aggiunti da Visual Studio forniscono una struttura di base per un'applicazione funzionante, completa con home page, informazioni sulla pagina, le pagine di accesso/disconnessione/registrazione degli account e una pagina di errore non gestita (tutti cablati e funzionanti).

### <a name="running-the-nerddinner-application"></a>Esecuzione dell'applicazione NerdDinner

È possibile eseguire il progetto scegliendo Debug **-&gt;Avvia debug** o **debug-&gt;avvia senza eseguire debug** voci di menu:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Verrà avviato il server Web ASP.NET incorporato incluso in Visual Studio ed eseguire l'applicazione:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Di seguito sono riportati i home page per il nuovo progetto (URL: "/") quando viene eseguito:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Facendo clic sulla scheda "about" viene visualizzata una pagina About (URL: "/Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Se si fa clic sul collegamento "Accedi" nella parte superiore destra, viene visitata una pagina di accesso (URL: "/Account/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Se non si dispone di un account di accesso, è possibile fare clic sul collegamento Register (URL: "/Account/Register") per crearne uno:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Il codice per implementare la funzionalità Home, about e logout/Register precedente è stato aggiunto per impostazione predefinita al momento della creazione del nuovo progetto. Verrà usato come punto di partenza dell'applicazione.

### <a name="testing-the-nerddinner-application"></a>Test dell'applicazione NerdDinner

Se si usa la versione Professional Edition o una versione successiva di Visual Studio 2008, è possibile usare il supporto integrato dell'IDE di testing unità in Visual Studio per testare il progetto:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Se si sceglie una delle opzioni precedenti, verrà aperto il riquadro "Risultati test" all'interno dell'IDE e verrà fornito lo stato superato/non superato nei 27 unit test inclusi nel nuovo progetto che coprono la funzionalità incorporata:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Più avanti in questa esercitazione parleremo di test automatizzati e aggiungono unit test aggiuntivi che coprono le funzionalità dell'applicazione implementate.

### <a name="next-step"></a>Passo successivo

A questo punto è disponibile una struttura di base dell'applicazione. Verrà ora [creato un database per archiviare i dati dell'applicazione](create-a-database.md).

> [!div class="step-by-step"]
> [Precedente](introducing-the-nerddinner-tutorial.md)
> [Successivo](create-a-database.md)
