---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Introduzione | Microsoft Docs
author: Rick-Anderson
description: WebMatrix non è più consigliato come Integrated Development Environment per Pagine Web ASP.NET. Usare Visual Studio o Visual Studio Code. Questa guida...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: bb863f8605e6f8faca3b285607b63a3e88e83012
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547103"
---
# <a name="getting-started"></a>Introduzione

di [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE[](~/includes/rp.md)]

> > [!NOTE] 
> > 
> > WebMatrix non è più consigliato come Integrated Development Environment per Pagine Web ASP.NET. Usare [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) o [Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Questa guida e questa applicazione offrono una panoramica di Pagine Web ASP.NET (versione 2 o successiva) e sintassi Razor, un framework leggero per la creazione di siti Web dinamici. Introduce inoltre WebMatrix, uno strumento per la creazione di pagine e siti.
> 
> **Level**: nuovo pagine Web ASP.NET.  
> **Competenze presupposte**: HTML, fogli di stile CSS di base.
> 
> Che verrà illustrato nella prima esercitazione del set:
> 
> - Che cos'è Pagine Web ASP.NET tecnologia e a cosa serve.
> - Cosa è WebMatrix.
> - Come installare tutti gli elementi.
> - Come creare un sito Web usando WebMatrix.
>   
> 
> Funzionalità/tecnologie discusse:
> 
> - Installazione guidata piattaforma Web Microsoft.
> - WebMatrix.
> - pagine *. cshtml*
>   
> 
> Mike Pope ha originariamente scritto questa esercitazione. Tom FitzMacken è stato aggiornato per Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 3

## <a name="what-should-you-know"></a>Cosa si può sapere?

Si presuppone che l'utente abbia familiarità con:

- **HTML**. Non è necessaria alcuna esperienza approfondita. Non verrà illustrato il codice HTML, ma non verrà usato alcun complesso. Verranno forniti i collegamenti alle esercitazioni HTML in cui riteniamo che siano utili.
- **Fogli di stile CSS**. Uguale a quello del codice HTML.
- **Idee di base sul database**. Se è stato usato un foglio di calcolo per i dati e sono stati ordinati e filtrati i dati, questo è il livello di esperienza che in genere si presuppone per questa serie di esercitazioni.

Si presuppone anche che l'utente sia interessato alla programmazione di base. Pagine Web ASP.NET utilizzare un linguaggio di programmazione C#denominato. Non è necessario avere alcun background nella programmazione, ma solo un interesse. Se in precedenza è stato scritto codice JavaScript in una pagina Web, si ha a che fare con molti retroscena.

Si noti che se si ha familiarità con la programmazione, è possibile che questo set di esercitazioni venga inizialmente spostato lentamente mentre i nuovi programmatori si avvicinano alla velocità. Mentre le prime esercitazioni sono state superate, tuttavia, sarà disponibile una programmazione di base per spiegare e le cose si sposteranno in un clip più rapido.

## <a name="what-do-you-need"></a>Quali sono gli elementi necessari?

Ecco cosa è necessario:

- Un computer che esegue Windows 8, Windows 7, Windows Server 2008 o Windows Server 2012.
- Una connessione Internet attiva.
- Privilegi di amministratore (necessari per il processo di installazione).

## <a name="what-is-aspnet-web-pages"></a>Che cos'è Pagine Web ASP.NET?

Pagine Web ASP.NET è un Framework che è possibile utilizzare per creare pagine Web dinamiche. Una semplice pagina Web HTML è statica; il contenuto è determinato dal markup HTML fisso presente nella pagina. Le pagine dinamiche come quelle create con Pagine Web ASP.NET consentono di creare rapidamente il contenuto della pagina usando il codice.

Le pagine dinamiche consentono di eseguire tutti i tipi di operazioni. È possibile chiedere a un utente di immettere l'input usando un modulo e quindi modificare la visualizzazione della pagina o l'aspetto. È possibile ottenere informazioni da un utente, salvarle in un database e quindi elencarle successivamente. È possibile inviare messaggi di posta elettronica dal sito. È possibile interagire con altri servizi sul Web (ad esempio, un servizio di mapping) e produrre pagine che integrano le informazioni provenienti da tali origini.

## <a name="what-is-webmatrix"></a>Che cos'è WebMatrix?

WebMatrix è uno strumento che integra un editor di pagine Web, un'utilità di database, un server Web per il test delle pagine e funzionalità per la pubblicazione di un sito Web su Internet. WebMatrix è gratuito ed è facile da installare e facile da usare. (Funziona anche per le pagine HTML semplici, nonché per altre tecnologie come PHP).

In *realtà non è necessario usare* WebMatrix per lavorare con pagine Web ASP.NET. È possibile creare pagine usando un editor di testo, ad esempio, e le pagine di test usando un server Web a cui si ha accesso. Tuttavia, WebMatrix lo rende molto semplice, quindi queste esercitazioni utilizzeranno WebMatrix in tutto.

## <a name="about-these-tutorials"></a>Informazioni sulle esercitazioni

Questo set di esercitazioni è un'introduzione a come usare Pagine Web ASP.NET. Questo set di esercitazioni introduttive include 9 esercitazioni totali. Fa parte di una serie di set di esercitazioni che consentono di Pagine Web ASP.NET novizio per la creazione di siti Web reali e dall'aspetto professionale.

Il primo set di esercitazioni si concentra sulla visualizzazione delle nozioni di base su come usare Pagine Web ASP.NET. Al termine, è possibile usare set di esercitazioni aggiuntivi che prevedono il punto in cui termina e che esplorano le pagine Web in modo più approfondito.

Le spiegazioni approfondite sono deliberatamente semplici. E ogni volta che viene visualizzato qualcosa, per questo set di esercitazioni è sempre necessario scegliere il modo più facile da comprendere. I set di esercitazioni successivi sono più approfonditi e mostrano un approccio più efficiente o più flessibile (anche più divertente). Tuttavia, per queste esercitazioni è necessario comprendere prima di tutto le nozioni di base.

Il set di esercitazioni appena avviato illustra le seguenti funzionalità di Pagine Web ASP.NET:

- Introduzione e recupero di tutti gli elementi installati. Nell'esercitazione che si sta leggendo.
- Nozioni di base sulla programmazione Pagine Web ASP.NET.
- Creazione di un database.
- Creazione ed elaborazione di un modulo di input dell'utente.
- Aggiunta, aggiornamento ed eliminazione di dati nel database.

## <a name="what-will-you-create"></a>Che cosa creerai?

Questo set di esercitazioni e versioni successive si riferiscono a un sito Web in cui è possibile elencare i film desiderati. Potrai immettere filmati, modificarli ed elencarli. Di seguito sono riportate alcune delle pagine che verranno create in questo set di esercitazioni. Il primo Mostra la pagina dell'elenco di film che verrà creata:

![App Movie finshed che mostra un elenco di film](getting-started/_static/image1.png)

Ecco la pagina che consente di aggiungere nuove informazioni sul film al sito:

![App film completata che mostra la pagina Aggiungi film](getting-started/_static/image2.png)

I set di esercitazioni successivi compilano su questo set e aggiungono altre funzionalità, ad esempio il caricamento di immagini, l'accesso degli utenti, l'invio di messaggi di posta elettronica e l'integrazione con i social media.

## <a name="see-this-app-running-on-azure"></a>Vedi questa app in esecuzione in Azure

Si desidera visualizzare il sito finito in esecuzione come app Web Live? È possibile distribuire una versione completa dell'app nell'account Azure semplicemente facendo clic sul pulsante seguente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Per distribuire questa soluzione in Azure, è necessario un account Azure. Se non si dispone già di un account, sono disponibili le opzioni seguenti:

- [Apri un account Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) gratuitamente: puoi ottenere crediti che puoi usare per provare i servizi di Azure a pagamento e, anche dopo che sono stati usati, puoi tenere l'account e usare i servizi di Azure gratuiti.
- [Attiva i vantaggi per gli abbonati MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : l'abbonamento MSDN ti offre crediti ogni mese che puoi usare per i servizi di Azure a pagamento.

## <a name="installing-everything"></a>Installazione di tutto

È possibile installare tutti gli elementi usando l'installazione guidata piattaforma Web da Microsoft. In pratica, si installa il programma di installazione e quindi lo si usa per installare tutto il resto.

Per utilizzare le pagine Web, è necessario disporre almeno di Windows XP con SP3 installato o Windows Server 2008 o versione successiva.

Nella [pagina pagine Web](../../../index.md) del sito Web ASP.NET fare clic su **Installa**.

![Sito Web ASP.NET che mostra il pulsante Installa&quot; WebMatrix &quot;](getting-started/_static/image3.png)

Viene richiesto di accettare le condizioni di licenza e l'informativa sulla privacy prima di installare WebMatrix.

![accetta il termine per iniziare l'installazione](getting-started/_static/image4.png)

Fare clic su **Esegui** per avviare l'installazione. Se si desidera salvare il programma di installazione, fare clic su **Salva** e quindi eseguire il programma di installazione dalla cartella in cui è stato salvato.

![](getting-started/_static/image5.png)

Viene visualizzata l'installazione guidata piattaforma Web, pronta per l'installazione di WebMatrix. Fare clic su **Installa**.

![](getting-started/_static/image6.png)

Il processo di installazione consente di specificare le informazioni da installare nel computer e di avviare il processo. A seconda di ciò che deve essere installato, il processo può richiedere da pochi istanti a diversi minuti. Selezionare **Accetto per accettare le** condizioni di licenza.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Al termine, il processo di installazione può avviare WebMatrix automaticamente. In caso contrario, in Windows, dal menu **Start** , avviare **Microsoft WebMatrix**.

Quando si avvia WebMatrix per la prima volta, si ha la possibilità di accedere a Microsoft Azure con il account Microsoft. Eseguendo l'accesso, si riceveranno 10 app Web gratuite tramite Azure. Queste app Web gratuite offrono un modo pratico per testare le app. Se non si ha già un account Azure, ma si dispone di un abbonamento MSDN, è possibile [attivare i vantaggi dell'abbonamento MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). In caso contrario, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere la pagina relativa alla [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Non è necessario eseguire l'accesso in questo momento per continuare con questa esercitazione. Se non si accede ora, sarà comunque possibile accedere in seguito. L'ultimo [argomento](publishing.md) di questa serie di esercitazioni illustra come distribuire il sito Web in Azure. per completare questo argomento, è quindi necessario eseguire l'accesso.

A questo punto, accedere con il account Microsoft o selezionare **non ora** nell'angolo inferiore destro.

![Accedi](getting-started/_static/image7.png)

Per iniziare, si creerà un sito Web vuoto e si aggiungerà una pagina. In un'esercitazione successiva di questo set verrà riprodotto uno dei modelli di sito Web predefiniti.

Nella finestra di avvio fare clic su **nuovo**.

![Schermata iniziale di WebMatrix](getting-started/_static/image8.png)

I modelli sono file e pagine predefiniti per diversi tipi di siti Web. Per visualizzare tutti i modelli disponibili per impostazione predefinita, selezionare l'opzione raccolta modelli.

![Selezionare la raccolta di modelli](getting-started/_static/image9.png)

Nella finestra di **avvio rapido** selezionare **sito vuoto** dal gruppo **ASP.NET** e assegnare al nuovo sito il nome "WebPagesMovies".

![Finestra Avvio rapido WebMatrix con un modello di sito vuoto selezionato](getting-started/_static/image10.png)

Fare clic su **Avanti**.

Se è stato effettuato l'accesso alla account Microsoft, si avrà la possibilità di creare il sito in Azure. In base al nome del sito, viene suggerito il nome predefinito **WebPagesMovies.azurewebsites.NET** . Tuttavia, il punto esclamativo indica che questo nome non è disponibile in Windows Azure. Per semplicità, selezionare **Ignora** per ignorare la creazione del sito Web in Azure al momento. Più avanti in questa serie, il sito verrà pubblicato in Azure.

![Crea sito di Azure](getting-started/_static/image11.png)

WebMatrix crea e apre il sito:

![Nuovo sito WebPagesMovies aperto in WebMatrix](getting-started/_static/image12.png)

Nella parte superiore sono disponibili una barra di accesso rapido e una barra multifunzione. In basso a sinistra viene visualizzato il selettore dell'area di lavoro in cui si passa da un'attività all'altra (**sito**, **file**, **database**, **report**). A destra è presente il riquadro del contenuto per l'editor e per i report. Nella parte inferiore verrà occasionalmente visualizzata una barra di notifica per i messaggi.

Verranno fornite ulteriori informazioni su WebMatrix e sulle relative funzionalità durante l'esercitazione.

## <a name="creating-a-web-page"></a>Creazione di una pagina Web

Per acquisire familiarità con WebMatrix e Pagine Web ASP.NET, è possibile creare una pagina semplice.

Nel selettore dell'area di lavoro selezionare l'area di lavoro **file** . Questa area di lavoro consente di usare file e cartelle. Il riquadro sinistro mostra la struttura di file del sito. La barra multifunzione cambia per visualizzare le attività correlate ai file.

![Area di lavoro file in WebMatrix](getting-started/_static/image13.png)

Sulla barra multifunzione fare clic sulla freccia in **nuovo** e quindi su **nuovo file**.

![Uso del comando &quot;nuovo&quot; nella barra multifunzione per creare un nuovo file](getting-started/_static/image14.png)

WebMatrix Visualizza un elenco di tipi di file. Selezionare **cshtml**e nella casella **nome** digitare "HelloWorld". Una pagina CSHTML è una pagina Pagine Web ASP.NET.

![Creazione di una nuova pagina CSHTML denominata HelloWorld. cshtml](getting-started/_static/image15.png)

Fare clic su **OK**.

WebMatrix crea la pagina e la apre nell'editor.

![La nuova pagina HelloWorld nell'editor di WebMatrix](getting-started/_static/image16.png)

Come si può notare, la pagina contiene principalmente il markup HTML comune, ad eccezione di un blocco nella parte superiore simile al seguente:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Per l'aggiunta di codice, come si vedrà a breve.

Si noti che le diverse parti della pagina &mdash; i nomi degli elementi, gli attributi e il testo, più il blocco nella parte superiore, sono tutti in colori diversi. Questa operazione è denominata *evidenziazione della sintassi*e rende più semplice la cancellazione di tutti gli elementi. Si tratta di una delle funzionalità che semplificano l'utilizzo delle pagine Web in WebMatrix.

Aggiungere contenuto per la `<head>` e `<body>` elementi come nell'esempio seguente. Se si desidera, è sufficiente copiare il blocco seguente e sostituire l'intera pagina esistente con questo codice.

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Nella barra di accesso rapido o nel menu **file** fare clic su **Salva**.

![Pulsante Salva nella barra di accesso rapido di WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Test della pagina

Nell'area di lavoro **file** , fare clic con il pulsante destro del mouse sulla pagina *HelloWorld. cshtml* , quindi fare clic su **Avvia nel browser**.

![Esecuzione di una pagina con il pulsante Esegui nella barra multifunzione di WebMatrix](getting-started/_static/image18.png)

WebMatrix avvia un server Web incorporato (IIS Express) che può essere usato per testare le pagine del computer. Senza IIS Express in WebMatrix, è necessario pubblicare la pagina in un server Web in un punto qualsiasi prima di poterla testare. La pagina viene visualizzata nel browser predefinito.

![&quot;Hello World&quot; pagina in esecuzione nel browser](getting-started/_static/image19.png)

Si noti che quando si esegue il test di una pagina in WebMatrix, l'URL nel browser è simile `http://localhost:33651/HelloWorld.cshtml.` il nome *localhost* fa riferimento a un server locale, vale a dire che la pagina viene gestita da un server Web che si trova nel computer. Come indicato, WebMatrix include un programma server Web denominato IIS Express che viene eseguito quando si avvia una pagina.

Il numero dopo *localhost* (ad esempio, *localhost: 33651*) si riferisce a un *numero di porta* nel computer. Questo è il numero del "canale" usato IIS Express per questo particolare sito Web. Il numero di porta viene selezionato in modo casuale dall'intervallo compreso tra 1024 e 65536 quando si crea il sito ed è diverso per ogni sito creato. Quando si esegue il test del proprio sito, il numero di porta sarà quasi certamente un numero diverso rispetto a 33561. Utilizzando una porta diversa per ogni sito Web, IIS Express possibile mantenerne i siti con cui sta parlando.

In un secondo momento, quando si pubblica il sito in un server Web pubblico, non viene più visualizzato *localhost* nell'URL. A questo punto, verrà visualizzato un URL più tipico, ad esempio `http://myhostingsite/mywebsite/HelloWorld.cshtml` o qualsiasi altra pagina. Verranno fornite ulteriori informazioni sulla pubblicazione di un sito più avanti in questa serie di esercitazioni.

## <a name="adding-some-server-side-code"></a>Aggiunta di codice sul lato server

Chiudere il browser e tornare alla pagina in WebMatrix.

Aggiungere una riga al blocco di codice in modo che abbia un aspetto simile al codice seguente:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Questo è un po' di codice Razor. È probabilmente chiaro che ottiene la data e l'ora correnti e inserisce tale valore in una *variabile* denominata `currentDateTime`. Ulteriori informazioni su sintassi Razor verranno fornite nell'esercitazione successiva.

Nel corpo della pagina, dopo l'elemento `<p>Hello World!</p>` aggiungere quanto segue:

[!code-html[Main](getting-started/samples/sample4.html)]

Questo codice ottiene il valore inserito nella variabile `currentDateTime` nella parte superiore e lo inserisce nel markup della pagina. Il carattere `@` contrassegna il codice Pagine Web ASP.NET nella pagina.

Eseguire di nuovo la pagina (WebMatrix Salva le modifiche prima di eseguire la pagina). Questa volta vengono visualizzati la data e l'ora nella pagina.

![&quot;Hello World&quot; pagina in esecuzione nel browser con una visualizzazione dell'ora generata dinamicamente](getting-started/_static/image20.png)

Attendere qualche minuto e quindi aggiornare la pagina nel browser. La visualizzazione della data e dell'ora è aggiornata.

Nel browser esaminare l'origine della pagina. Il markup è simile al seguente:

[!code-html[Main](getting-started/samples/sample5.html)]

Si noti che il blocco `@{ }` nella parte superiore non è presente. Si noti inoltre che la visualizzazione di data e ora Mostra una stringa di caratteri effettiva (`1/18/2012 2:49:50 PM` o qualsiasi altra), non `@currentDateTime` come nella pagina *. cshtml* . Ciò che è successo è che, quando è stata eseguita la pagina, ASP.NET ha elaborato tutto il codice (in questo caso pochissimo) contrassegnato con `@`. Il codice produce l'output e tale output è stato inserito nella pagina.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Questo è il Pagine Web ASP.NET

Quando si legge che Pagine Web ASP.NET produce contenuto Web dinamico, ciò che si è visto qui è l'idea. La pagina appena creata contiene lo stesso markup HTML illustrato in precedenza. Può inoltre contenere codice in grado di eseguire qualsiasi tipo di attività. In questo esempio, è stata la semplice attività di ottenere la data e l'ora correnti. Come si è visto, è possibile intercalare il codice con il codice HTML per produrre output nella pagina. Quando un utente richiede una pagina con *estensione cshtml* nel browser, ASP.NET elabora la pagina mentre è ancora in mano al server Web. ASP.NET inserisce l'output del codice, se presente, nella pagina come HTML. Al termine dell'elaborazione del codice, ASP.NET invia la pagina risultante al browser. Tutto il browser ottenuto è HTML. Ecco un diagramma:

![Flusso concettuale del modo in cui ASP.NET genera in modo dinamico il codice HTML](getting-started/_static/image21.png)

L'idea è semplice, ma ci sono molte attività interessanti che il codice è in grado di eseguire e sono disponibili molti modi interessanti per aggiungere dinamicamente contenuto HTML alla pagina. Le pagine ASP.NET *. cshtml* , come qualsiasi pagina HTML, possono includere anche codice eseguito nel browser stesso (codice JavaScript e jQuery). Si esamineranno tutti questi elementi in questo set di esercitazioni e in quelli successivi.

## <a name="coming-up-next"></a>Prossimi

Nell'esercitazione successiva di questa serie si esplorerà Pagine Web ASP.NET programmazione.

## <a name="additional-resources"></a>Risorse aggiuntive

[Creare un sito web ASP.NET da zero](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Si tratta di un'esercitazione che descrive in modo specifico l'uso di WebMatrix (non Pagine Web ASP.NET). Questa esercitazione illustra in dettaglio alcune delle funzionalità aggiuntive di WebMatrix che non verranno trattate in questa esercitazione.

> [!div class="step-by-step"]
> [avanti](intro-to-web-pages-programming.md)
