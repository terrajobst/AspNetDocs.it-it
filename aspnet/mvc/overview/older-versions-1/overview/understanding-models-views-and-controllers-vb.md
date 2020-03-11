---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
title: Informazioni su modelli, visualizzazioni e controller (VB) | Microsoft Docs
author: StephenWalther
description: Confuso sui modelli, le visualizzazioni e i controller? In questa esercitazione Stephen Walther presenta le diverse parti di un'applicazione MVC ASP.NET.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: a106374a-5e74-4fd0-9ac0-1a32280e5d0d
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
msc.type: authoredcontent
ms.openlocfilehash: cc7988e0c9802e8cd376396eb5da15b5393d6088
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600450"
---
# <a name="understanding-models-views-and-controllers-vb"></a>Informazioni su modelli, visualizzazioni e controller (VB)

di [Stephen Walther](https://github.com/StephenWalther)

> Confuso sui modelli, le visualizzazioni e i controller? In questa esercitazione Stephen Walther presenta le diverse parti di un'applicazione MVC ASP.NET.

Questa esercitazione offre una panoramica di alto livello di modelli, visualizzazioni e controller MVC ASP.NET. In altre parole, vengono illustrati gli M', V'e c'in ASP.NET MVC.

Dopo aver letto questa esercitazione, è necessario comprendere in che modo le diverse parti di un'applicazione MVC ASP.NET interagiscono. È anche necessario comprendere in che modo l'architettura di un'applicazione MVC ASP.NET differisce da un'applicazione Web Form ASP.NET o da un'applicazione Active Server Pages.

## <a name="the-sample-aspnet-mvc-application"></a>Applicazione MVC ASP.NET di esempio

Il modello predefinito di Visual Studio per la creazione di applicazioni Web MVC ASP.NET include un'applicazione di esempio estremamente semplice che può essere usata per comprendere le diverse parti di un'applicazione MVC ASP.NET. Questa semplice applicazione verrà sfruttata in questa esercitazione.

Per creare una nuova applicazione MVC ASP.NET con il modello MVC, avviare Visual Studio 2008 e selezionare il file dell'opzione di menu nuovo progetto (vedere la figura 1). Nella finestra di dialogo nuovo progetto selezionare il linguaggio di programmazione preferito in tipi di progetto ( C#Visual Basic o) e selezionare **applicazione Web MVC ASP.NET** in modelli. Fare clic sul pulsante OK.

[![finestra di dialogo nuovo progetto](understanding-models-views-and-controllers-vb/_static/image1.jpg)](understanding-models-views-and-controllers-vb/_static/image1.png)

**Figura 01**: finestra di dialogo nuovo progetto ([fare clic per visualizzare l'immagine con dimensioni complete](understanding-models-views-and-controllers-vb/_static/image2.png))

Quando si crea una nuova applicazione MVC ASP.NET, viene visualizzata la finestra di dialogo **Crea progetto di unit test** (vedere la figura 2). Questa finestra di dialogo consente di creare un progetto separato nella soluzione per il test dell'applicazione ASP.NET MVC. Selezionare l'opzione **No, non creare un progetto unit test** e fare clic sul pulsante **OK** .

[Finestra di dialogo ![Crea unit test](understanding-models-views-and-controllers-vb/_static/image2.jpg)](understanding-models-views-and-controllers-vb/_static/image3.png)

**Figura 02**: finestra di dialogo Crea unit test ([fare clic per visualizzare l'immagine con dimensioni complete](understanding-models-views-and-controllers-vb/_static/image4.png))

Dopo la creazione della nuova applicazione MVC ASP.NET. Nella finestra Esplora soluzioni vengono visualizzati diversi file e cartelle. In particolare, verranno visualizzate tre cartelle denominate modelli, visualizzazioni e controller. Come si può intuire dai nomi delle cartelle, queste cartelle contengono i file per l'implementazione di modelli, visualizzazioni e controller.

Se si espande la cartella controller, verrà visualizzato un file denominato AccountController. vb e un file denominato HomeController. vb. Se si espande la cartella Views, verranno visualizzate tre sottocartelle denominate account, Home e Shared. Se si espande la cartella Home, verranno visualizzati due file aggiuntivi denominati about. aspx e index. aspx (vedere la figura 3). Questi file costituiscono l'applicazione di esempio inclusa nel modello MVC ASP.NET predefinito.

[![finestra Esplora soluzioni](understanding-models-views-and-controllers-vb/_static/image3.jpg)](understanding-models-views-and-controllers-vb/_static/image5.png)

**Figura 03**: finestra Esplora soluzioni ([fare clic per visualizzare l'immagine con dimensioni complete](understanding-models-views-and-controllers-vb/_static/image6.png))

È possibile eseguire l'applicazione di esempio selezionando l'opzione di menu **debug, Avvia debug**. In alternativa, è possibile premere il tasto F5.

Quando si esegue un'applicazione ASP.NET per la prima volta, viene visualizzata la finestra di dialogo nella figura 4 che consiglia di abilitare la modalità di debug. Fare clic sul pulsante OK. l'applicazione viene eseguita.

[![finestra di dialogo debug non abilitato](understanding-models-views-and-controllers-vb/_static/image4.jpg)](understanding-models-views-and-controllers-vb/_static/image7.png)

**Figura 04**: finestra di dialogo debug non abilitato ([fare clic per visualizzare l'immagine con dimensioni complete](understanding-models-views-and-controllers-vb/_static/image8.png))

Quando si esegue un'applicazione MVC ASP.NET, Visual Studio avvia l'applicazione nel Web browser. L'applicazione di esempio è costituita solo da due pagine: la pagina di indice e la pagina informazioni su. Quando l'applicazione viene avviata per la prima volta, viene visualizzata la pagina di indice (vedere la figura 5). È possibile passare alla pagina about facendo clic sul collegamento di menu in alto a destra nell'applicazione.

[![pagina di indice](understanding-models-views-and-controllers-vb/_static/image5.jpg)](understanding-models-views-and-controllers-vb/_static/image9.png)

**Figura 05**: pagina di indice ([fare clic per visualizzare l'immagine con dimensioni complete](understanding-models-views-and-controllers-vb/_static/image10.png))

Si notino gli URL nella barra degli indirizzi del browser. Ad esempio, quando si fa clic sul collegamento di menu about, l'URL nella barra degli indirizzi del browser diventa **/Home/About**.

Se si chiude la finestra del browser e si torna a Visual Studio, non sarà possibile trovare un file con il percorso Home/About. I file non esistono. Come è possibile?

## <a name="a-url-does-not-equal-a-page"></a>Un URL non è uguale a una pagina

Quando si compila un'applicazione Web Form ASP.NET tradizionale o un'applicazione Active Server Pages, esiste una corrispondenza uno-a-uno tra un URL e una pagina. Se si richiede una pagina denominata SomePage. aspx dal server, è stato migliore una pagina sul disco denominata SomePage. aspx. Se il file SomePage. aspx non esiste, si ottiene un errore di **404-pagina non trovato** .

Quando si compila un'applicazione MVC ASP.NET, al contrario, non esiste alcuna corrispondenza tra l'URL digitato nella barra degli indirizzi del browser e i file che si trovano nell'applicazione. In un'applicazione MVC ASP.NET un URL corrisponde a un'azione del controller invece che a una pagina su disco.

In un'applicazione ASP.NET o ASP tradizionale, le richieste del browser vengono mappate alle pagine. In un'applicazione MVC ASP.NET, al contrario, le richieste del browser vengono mappate alle azioni del controller. Un'applicazione Web Form ASP.NET è incentrata sul contenuto. Un'applicazione MVC ASP.NET, al contrario, è incentrata sulla logica dell'applicazione.

## <a name="understanding-aspnet-routing"></a>Informazioni sul routing ASP.NET

Viene eseguito il mapping di una richiesta del browser a un'azione del controller tramite una funzionalità del framework ASP.NET denominato *ASP.NET routing*. Il routing ASP.NET viene usato dal framework MVC ASP.NET per *indirizzare* le richieste in ingresso alle azioni del controller.

Il routing ASP.NET usa una tabella di route per gestire le richieste in ingresso. Questa tabella di route viene creata all'avvio iniziale dell'applicazione Web. La tabella di route è impostata nel file Global. asax. Il file Global. asax MVC predefinito è contenuto nel listato 1.

**Listato 1-Global. asax**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample1.vb)]

Quando un'applicazione ASP.NET viene avviata per la prima volta, viene chiamato il metodo Start () dell'\_applicazione. Nel listato 1, questo metodo chiama il metodo RegisterRoutes () e il metodo RegisterRoutes () crea la tabella di route predefinita.

La tabella di route predefinita è costituita da una route. Questa route predefinita interrompe tutte le richieste in ingresso in tre segmenti (un segmento URL è qualsiasi elemento tra le barre). Il primo segmento viene mappato a un nome di controller, il secondo segmento viene mappato a un nome di azione e il segmento finale viene mappato a un parametro passato all'azione denominata ID.

Si consideri ad esempio l'URL seguente:

/Product/Details/3

Questo URL viene analizzato in tre parametri come il seguente:

Controller = prodotto

Azione = dettagli

ID = 3

La route predefinita definita nel file Global. asax include i valori predefiniti per tutti e tre i parametri. Il controller predefinito è Home, l'azione predefinita è index e l'ID predefinito è una stringa vuota. Tenendo presenti queste impostazioni predefinite, valutare la modalità di analisi dell'URL seguente:

/Employee

Questo URL viene analizzato in tre parametri come il seguente:

Controller = dipendente

Action = indice

ID =

Infine, se si apre un'applicazione MVC ASP.NET senza specificare alcun URL (ad esempio, `http://localhost`), l'URL viene analizzato come segue:

controller = Home

Action = indice

ID =

La richiesta viene instradata all'azione index () nella classe HomeController.

## <a name="understanding-controllers"></a>Informazioni sui controller

Un controller è responsabile del controllo del modo in cui un utente interagisce con un'applicazione MVC. Un controller contiene la logica di controllo di flusso per un'applicazione MVC ASP.NET. Un controller determina quale risposta restituire a un utente quando un utente effettua una richiesta del browser.

Un controller è semplicemente una classe (ad esempio, un Visual Basic o C# una classe). L'applicazione ASP.NET MVC di esempio include un controller denominato HomeController. vb che si trova nella cartella Controllers. Il contenuto del file HomeController. vb viene riprodotto nel listato 2.

**Listato 2-HomeController.cs**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample2.vb)]

Si noti che HomeController dispone di due metodi denominati index () e About (). Questi due metodi corrispondono alle due azioni esposte dal controller. L'URL/Home/Index richiama il metodo HomeController. index () e l'URL/Home/About richiama il metodo HomeController. about ().

Qualsiasi metodo pubblico in un controller viene esposto come azione del controller. È necessario prestare attenzione. Ciò significa che qualsiasi metodo pubblico contenuto in un controller può essere richiamato da qualsiasi utente con accesso a Internet immettendo l'URL corretto in un browser.

## <a name="understanding-views"></a>Informazioni sulle viste

Le due azioni del controller esposte dalla classe HomeController, index () e About (), restituiscono entrambe una vista. Una vista contiene il markup HTML e il contenuto inviato al browser. Una visualizzazione è l'equivalente di una pagina quando si utilizza un'applicazione MVC ASP.NET.

È necessario creare le visualizzazioni nella posizione corretta. L'azione HomeController. index () restituisce una visualizzazione che si trova nel percorso seguente:

\Views\Home\Index.aspx

L'azione HomeController. about () restituisce una visualizzazione che si trova nel percorso seguente:

\Views\Home\About.aspx

In generale, se si desidera restituire una visualizzazione per un'azione del controller, è necessario creare una sottocartella nella cartella Views con lo stesso nome del controller. All'interno della sottocartella è necessario creare un file con estensione aspx con lo stesso nome dell'azione del controller.

Il file nel listato 3 contiene la visualizzazione About. aspx.

**Listato 3-about. aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-vb/samples/sample3.aspx)]

Se si ignora la prima riga del listato 3, la maggior parte del resto della vista è costituita da HTML standard. È possibile modificare il contenuto della visualizzazione immettendo il codice HTML desiderato.

Una visualizzazione è molto simile a una pagina in Active Server pagine o Web Form ASP.NET. Una vista può contenere contenuto HTML e script. È possibile scrivere gli script nel linguaggio di programmazione .NET preferito, C# ad esempio o Visual Basic .NET. Usare gli script per visualizzare contenuto dinamico, ad esempio dati del database.

## <a name="understanding-models"></a>Informazioni sui modelli

Sono stati discussi i controller e sono state illustrate le visualizzazioni. L'ultimo argomento che è necessario discutere è i modelli. Che cos'è un modello MVC?

Un modello MVC contiene tutta la logica dell'applicazione che non è contenuta in una vista o un controller. Il modello deve contenere tutta la logica di business, la logica di convalida e la logica di accesso al database dell'applicazione. Se ad esempio si utilizza Microsoft Entity Framework per accedere al database, è necessario creare le classi Entity Framework (il file con estensione edmx) nella cartella Models.

Una vista deve contenere solo la logica relativa alla generazione dell'interfaccia utente. Un controller deve contenere solo il minimo indispensabile per la logica necessaria per restituire la visualizzazione corretta o reindirizzare l'utente a un'altra azione (controllo di flusso). Tutto il resto dovrebbe essere contenuto nel modello.

In generale, è consigliabile cercare i modelli FAT e i controller skinny. I metodi del controller devono contenere solo poche righe di codice. Se un'azione del controller diventa troppo grassa, è consigliabile provare a trasferire la logica in una nuova classe nella cartella Models (modelli).

## <a name="summary"></a>Riepilogo

Questa esercitazione ha fornito una panoramica di alto livello delle diverse parti di un'applicazione Web MVC ASP.NET. Si è appreso come il routing ASP.NET esegue il mapping delle richieste del browser in ingresso a specifiche azioni del controller. Si è appreso come i controller orchestrano il modo in cui le visualizzazioni vengono restituite al browser. Infine, si è appreso come i modelli contengano la logica di accesso al database, la convalida e l'azienda.
