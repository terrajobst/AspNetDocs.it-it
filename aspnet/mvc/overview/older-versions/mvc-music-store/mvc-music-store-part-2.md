---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: controller | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 2 illustra i controller.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559878"
---
# <a name="part-2-controllers"></a>Parte 2: controller

di [Jon Galloway](https://github.com/jongalloway)

> Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.  
>   
> Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 2 illustra i controller.

Con i framework Web tradizionali, gli URL in ingresso vengono in genere mappati a file su disco. Ad esempio, una richiesta per un URL come "/Products.aspx" o "/Products.php" potrebbe essere elaborata da un file "Products. aspx" o "Products. php".

I framework MVC basati sul Web mappano gli URL al codice server in modo leggermente diverso. Anziché eseguire il mapping degli URL in ingresso ai file, eseguono invece il mapping degli URL ai metodi nelle classi. Queste classi sono denominate "controller" e sono responsabili dell'elaborazione delle richieste HTTP in ingresso, della gestione dell'input dell'utente, del recupero e del salvataggio dei dati e della determinazione della risposta da inviare al client (visualizzazione HTML, download di un file, Reindirizzamento a un altro URL e così via).

## <a name="adding-a-homecontroller"></a>Aggiunta di un HomeController

Si inizierà l'applicazione MVC Music Store aggiungendo una classe controller che gestirà gli URL alla Home page del sito. Si seguiranno le convenzioni di denominazione predefinite di ASP.NET MVC e lo si chiamerà HomeController.

Fare clic con il pulsante destro del mouse sulla cartella "controller" all'interno del Esplora soluzioni e selezionare "Aggiungi", quindi il "controller". comando

![](mvc-music-store-part-2/_static/image1.jpg)

Verrà visualizzata la finestra di dialogo "Aggiungi controller". Assegnare al controller il nome "HomeController" e premere il pulsante Aggiungi.

![](mvc-music-store-part-2/_static/image1.png)

Verrà creato un nuovo file, HomeController.cs, con il codice seguente:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Per iniziare nel modo più semplice possibile, sostituire il metodo index con un metodo semplice che restituisce solo una stringa. Verranno apportate due modifiche:

- Modificare il metodo in modo che restituisca una stringa anziché un ActionResult
- Modificare l'istruzione return per restituire "Hello from Home"

Il metodo dovrebbe ora essere simile al seguente:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Esecuzione dell'applicazione

A questo punto, eseguire il sito. È possibile avviare il server Web e provare il sito usando uno dei seguenti elementi:

- Scegliere la voce di menu debug ⇨ Avvia debug
- Fare clic sul pulsante freccia verde sulla barra degli strumenti ![](mvc-music-store-part-2/_static/image2.jpg)
- Usare il tasto di scelta rapida F5.

L'uso di uno dei passaggi precedenti compilerà il progetto, quindi comporterà l'avvio del Server di sviluppo ASP.NET incorporato in Visual Web Developer. Verrà visualizzata una notifica nell'angolo inferiore dello schermo per indicare che l'Server di sviluppo ASP.NET è stata avviata e visualizzerà il numero di porta in cui è in esecuzione.

![](mvc-music-store-part-2/_static/image2.png)

In Visual Web Developer verrà aperta automaticamente una finestra del browser il cui URL punta al server Web. Questo consentirà di provare rapidamente l'applicazione Web:

![](mvc-music-store-part-2/_static/image3.png)

Ok, era piuttosto rapido: abbiamo creato un nuovo sito Web, abbiamo aggiunto una funzione a tre righe e abbiamo ottenuto testo in un browser. Non è una scienza missilistica, ma si tratta di un inizio.

*Nota: Visual Web Developer include il Server di sviluppo ASP.NET, che eseguirà il sito Web in un numero "porta" libero casuale. Nella schermata precedente il sito è in esecuzione in `http://localhost:26641/`, quindi usa la porta 26641. Il numero di porta sarà diverso. Quando si parla di URL come/Store/Browse in questa esercitazione, questa operazione verrà completata dopo il numero di porta. Supponendo un numero di porta 26641, l'esplorazione di/Store/Browse significherà l'esplorazione `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Aggiunta di un StoreController

È stato aggiunto un semplice HomeController che implementa la Home page del sito. A questo punto, aggiungere un altro controller che verrà usato per implementare la funzionalità di esplorazione di Music Store. Il controller dello Store supporterà tre scenari:

- Pagina di presentazione dei generi musicali nel nostro archivio musica
- Pagina Sfoglia che elenca tutti gli album musicali di un determinato genere
- Pagina dei dettagli che mostra le informazioni su uno specifico album musicale

Si inizierà aggiungendo una nuova classe StoreController. Se non è già stato fatto, arrestare l'esecuzione dell'applicazione chiudendo il browser o selezionando la voce di menu debug ⇨ Interrompi debug.

Aggiungere ora un nuovo StoreController. Analogamente a quanto avviene con HomeController, questa operazione viene eseguita facendo clic con il pulsante destro del mouse sulla cartella "Controllers" all'interno del Esplora soluzioni e scegliendo la voce di menu Aggiungi&gt;controller

![](mvc-music-store-part-2/_static/image4.png)

Il nuovo StoreController dispone già di un metodo "index". Questo metodo "index" verrà usato per implementare la pagina di presentazione in cui sono elencati tutti i generi presenti nel Music Store. Verranno anche aggiunti altri due metodi per implementare gli altri due scenari che si vuole gestire con il StoreController: browse and details.

Questi metodi (index, browse e Details) all'interno del controller sono denominati "azioni del controller" e, come già visto con il metodo di azione HomeController. index (), il loro lavoro consiste nel rispondere alle richieste URL e, in generale, determinare il contenuto deve essere restituito al browser o all'utente che ha richiamato l'URL.

Verrà avviata l'implementazione di StoreController modificando il metodo theIndex () per restituire la stringa "Hello from Store. index ()" e si aggiungeranno metodi simili per Browse () e Details ():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Eseguire nuovamente il progetto ed esplorare gli URL seguenti:

- /Store
- /Store/Browse
- /Store/Details

L'accesso a questi URL richiama i metodi di azione all'interno del controller e restituiscono risposte stringa:

![](mvc-music-store-part-2/_static/image5.png)

Questo è un ottimo, ma si tratta solo di stringhe costanti. È quindi opportuno renderli dinamici, in modo da ottenere informazioni dall'URL e visualizzarle nell'output della pagina.

Prima di tutto verrà modificato il metodo di azione browse per recuperare un valore QueryString dall'URL. È possibile eseguire questa operazione aggiungendo un parametro "genre" al metodo di azione. Quando si esegue questa operazione, ASP.NET MVC passa automaticamente i parametri QueryString o post del form denominati "genre" al metodo di azione quando viene richiamato.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Nota: il metodo di utilità HttpUtility. HtmlEncode viene usato per purificare l'input dell'utente. In questo modo si impedisce agli utenti di inserire JavaScript nella visualizzazione con un collegamento come/Store/Browse? Genre =&lt;script&gt;finestra. location ='http://hackersite.com'&lt;/script&gt;.*

Passiamo ora a/Store/Browse? Genere = discoteca

![](mvc-music-store-part-2/_static/image6.png)

Modificare quindi l'azione dettagli per leggere e visualizzare un parametro di input denominato ID. A differenza del metodo precedente, il valore ID non verrà incorporato come parametro QueryString. Verrà invece incorporata direttamente all'interno dell'URL. Ad esempio:/Store/Details/5.

ASP.NET MVC consente di eseguire facilmente questa operazione senza dover configurare alcun elemento. La convenzione di routing predefinita di ASP.NET MVC consiste nel considerare il segmento di un URL dopo il nome del metodo di azione come parametro denominato "ID". Se il metodo di azione include un parametro denominato ID, ASP.NET MVC passa automaticamente il segmento dell'URL all'utente come parametro.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Eseguire l'applicazione e passare a/Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Ricapitoliamo ora quanto segue:

- È stato creato un nuovo progetto MVC ASP.NET in Visual Web Developer
- È stata illustrata la struttura di cartelle di base di un'applicazione MVC ASP.NET
- Abbiamo appreso come eseguire il nostro sito Web usando il Server di sviluppo ASP.NET
- Sono state create due classi controller: HomeController e StoreController
- Sono stati aggiunti metodi di azione ai controller che rispondono alle richieste URL e restituiscono testo al browser

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-1.md)
> [Successivo](mvc-music-store-part-3.md)
