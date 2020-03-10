---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
title: Creazione di un layout a livello di sito tramite leC#pagine master () | Microsoft Docs
author: rick-anderson
description: In questa esercitazione vengono illustrate le nozioni fondamentali della pagina master. In particolare, che cosa sono le pagine master, in che modo crea una pagina master, che cosa sono i titolari del contenuto, in che modo viene eseguita una CR...
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 78f8d194-03b9-44a5-8255-90e7cd1c2ee1
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a5e85c443a2a3642ec185ab1897c43cdb2ab1f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78625391"
---
# <a name="creating-a-site-wide-layout-using-master-pages-c"></a>Creazione di un layout a livello di sito tramite le pagine master (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_CS.pdf)

> In questa esercitazione vengono illustrate le nozioni fondamentali della pagina master. In particolare, cosa sono le pagine master, in che modo crea una pagina master, che cosa sono i titolari dei contenuti, come crea una pagina ASP.NET che usa una pagina master, come la modifica della pagina master viene riflessa automaticamente nelle pagine di contenuto associate e così via.

## <a name="introduction"></a>Introduzione

Un attributo di un sito web ben progettato è un layout di pagina coerente a livello di sito. Si prenda ad esempio il sito Web www.asp.net. Al momento della stesura di questo articolo, ogni pagina ha lo stesso contenuto nella parte superiore e inferiore della pagina. Come illustrato nella figura 1, nella parte superiore di ogni pagina viene visualizzata una barra grigia con un elenco di community Microsoft. Sotto questo è il logo del sito, l'elenco di lingue in cui è stato convertito il sito e le sezioni principali: Home, introduzione, informazioni, download e così via. Allo stesso modo, nella parte inferiore della pagina sono incluse informazioni sull'annuncio su www.asp.net, una dichiarazione sul copyright e un collegamento all'informativa sulla privacy.

[![il sito Web www.asp.net usa un aspetto coerente in tutte le pagine](creating-a-site-wide-layout-using-master-pages-cs/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image1.png)

<strong>Figura 01</strong>: il sito Web www.ASP.NET usa un aspetto coerente in tutte le pagine ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-site-wide-layout-using-master-pages-cs/_static/image3.png))

Un altro attributo di un sito ben progettato è la facilità con cui è possibile modificare l'aspetto del sito. La figura 1 Mostra la Home page di www.asp.net a partire dal 2008 marzo, ma tra ora e la pubblicazione di questa esercitazione, l'aspetto potrebbe essere cambiato. Forse le voci di menu lungo la parte superiore si espanderanno per includere una nuova sezione per il framework MVC. O forse una progettazione radicalmente nuova con colori, tipi di carattere e layout diversi verrà svelata. L'applicazione di tali modifiche all'intero sito deve essere un processo veloce e semplice che non richiede la modifica delle migliaia di pagine Web che costituiscono il sito.

La creazione di un modello di pagina a livello di sito in ASP.NET è possibile tramite l'utilizzo di *pagine master*. In breve, una pagina master è un tipo speciale di pagina ASP.NET che definisce il markup comune tra tutte le pagine di *contenuto* , nonché le aree personalizzabili in base a una pagina di contenuto. Una pagina di contenuto è una pagina di ASP.NET associata alla pagina master. Ogni volta che viene modificato il layout o la formattazione di una pagina master, l'output di tutte le relative pagine di contenuto viene immediatamente aggiornato, il che rende più semplice l'aggiornamento e la distribuzione di un singolo file (ovvero la pagina master).

Questa è la prima esercitazione di una serie di esercitazioni che esplorano l'uso di pagine master. Nel corso di questa serie di esercitazioni:

- Esaminare la creazione di pagine master e le pagine di contenuto associate,
- Illustra una serie di suggerimenti, trucchi e trap,
- Identificare i problemi comuni della pagina master ed esplorare le soluzioni alternative,
- Vedere come accedere alla pagina master da una pagina di contenuto e viceversa,
- Informazioni su come specificare la pagina master di una pagina di contenuto in fase di esecuzione e
- Altri argomenti avanzati della pagina master.

Queste esercitazioni sono incentrate su concise e forniscono istruzioni dettagliate con numerose schermate per illustrare il processo in modo visivo. Ogni esercitazione è disponibile in C# e Visual Basic versioni e include un download del codice completo usato.

Questa esercitazione introduttiva inizia con una panoramica delle nozioni di base della pagina master. Viene illustrato il funzionamento delle pagine master, viene illustrato come creare una pagina master e le pagine di contenuto associate mediante Visual Web Developer e come visualizzare immediatamente le modifiche apportate a una pagina master nelle pagine di contenuto. Ecco come procedere.

## <a name="understanding-how-master-pages-work"></a>Informazioni sul funzionamento delle pagine master

La creazione di un sito Web con un layout di pagina coerente a livello di sito richiede che ogni pagina Web emetta un markup di formattazione comune oltre al contenuto personalizzato. Ad esempio, mentre ogni esercitazione o post del forum su www.asp.net ha il proprio contenuto univoco, ognuna di queste pagine esegue anche il rendering di una serie di elementi di `<div>` comuni che visualizzano i collegamenti alla sezione di primo livello: Home, introduzione, informazioni e così via.

Sono disponibili diverse tecniche per la creazione di pagine Web con un aspetto coerente. Un approccio ingenuo consiste nel copiare e incollare semplicemente il markup del layout comune in tutte le pagine Web, ma questo approccio presenta diversi svantaggi. Per i principianti, ogni volta che viene creata una nuova pagina, è necessario ricordarsi di copiare e incollare il contenuto condiviso nella pagina. Le operazioni di copia e incolla sono pronte per l'errore perché è possibile copiare accidentalmente solo un subset del markup condiviso in una nuova pagina. Per la maggior parte, questo approccio consente di sostituire l'aspetto esistente a livello di sito con un nuovo dolore reale, perché ogni singola pagina del sito deve essere modificata per poter usare il nuovo aspetto.

Prima di ASP.NET versione 2,0, gli sviluppatori di pagine posizionavano spesso markup comune nei [controlli utente](https://msdn.microsoft.com/library/y6wb1a0e.aspx) e quindi aggiungevano questi controlli utente a ogni pagina. Questo approccio richiede che lo sviluppatore della pagina ricordi di aggiungere manualmente i controlli utente a ogni nuova pagina, ma consentiti per semplificare le modifiche a livello di sito perché quando si aggiorna il markup comune è necessario modificare solo i controlli utente. Sfortunatamente, Visual Studio .NET 2002 e 2003: le versioni di Visual Studio usate per creare le applicazioni ASP.NET 1. x hanno eseguito il rendering dei controlli utente nella visualizzazione progettazione come caselle grigie. Di conseguenza, gli sviluppatori di pagine che usano questo approccio non godono di un ambiente WYSIWYG in fase di progettazione.

Gli svantaggi dell'uso dei controlli utente sono stati risolti in ASP.NET versione 2,0 e Visual Studio 2005 con l'introduzione di *pagine master*. Una pagina master è un tipo speciale di pagina ASP.NET che definisce il markup a livello di sito e le *aree* in cui le *pagine di contenuto* associate definiscono il markup personalizzato. Come si vedrà nel passaggio 1, queste aree sono definite dai controlli ContentPlaceHolder. Il controllo ContentPlaceHolder indica semplicemente una posizione nella gerarchia dei controlli della pagina master in cui è possibile inserire contenuto personalizzato da una pagina di contenuto.

> [!NOTE]
> I concetti di base e la funzionalità delle pagine master non sono stati modificati rispetto alla versione 2,0 di ASP.NET. Tuttavia, Visual Studio 2008 offre supporto in fase di progettazione per le pagine master annidate, una funzionalità che non era disponibile in Visual Studio 2005. In un'esercitazione futura si esamineranno le pagine master nidificate.

La figura 2 Mostra l'aspetto della pagina master per www.asp.net. Si noti che la pagina master definisce il layout comune a livello di sito, ovvero il markup in alto, in basso e a destra di ogni pagina, nonché un ContentPlaceHolder nella parte centrale sinistra, in cui si trova il contenuto univoco per ogni singola pagina Web.

![Una pagina master definisce il layout a livello di sito e le aree modificabili in base a una pagina di contenuto](creating-a-site-wide-layout-using-master-pages-cs/_static/image4.png)

**Figura 02**: una pagina master definisce il layout a livello di sito e le aree modificabili in base a una pagina di contenuto.

Una volta definita, una pagina master può essere associata a nuove pagine ASP.NET tramite il segno di spunta di una casella di controllo. Queste pagine di ASP.NET, denominate pagine di contenuto, includono un controllo contenuto per ognuno dei controlli ContentPlaceHolder della pagina master. Quando la pagina contenuto viene visitata tramite un browser, il motore ASP.NET crea la gerarchia dei controlli della pagina master e inserisce la gerarchia dei controlli della pagina contenuto nelle posizioni appropriate. Viene eseguito il rendering di questa gerarchia di controlli combinata e il codice HTML risultante viene restituito al browser dell'utente finale. Di conseguenza, la pagina contenuto genera sia il markup comune definito nella propria pagina master all'esterno dei controlli ContentPlaceHolder che il markup specifico della pagina definito all'interno dei propri controlli contenuto. Nella figura 3 viene illustrato questo concetto.

[![il markup della pagina richiesta viene fuso nella pagina master](creating-a-site-wide-layout-using-master-pages-cs/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image5.png)

**Figura 03**: il markup della pagina richiesta viene fuso nella pagina master ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-site-wide-layout-using-master-pages-cs/_static/image7.png))

Ora che è stata illustrata la procedura per le pagine master, è opportuno creare una pagina master e le pagine di contenuto associate tramite Visual Web Developer.

> [!NOTE]
> Per raggiungere il maggior pubblico possibile, il sito Web ASP.NET creato in questa serie di esercitazioni verrà creato usando ASP.NET 3,5 con la versione gratuita di Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)di Microsoft. Se non è ancora stato eseguito l'aggiornamento a ASP.NET 3,5, i concetti illustrati in queste esercitazioni funzionano ugualmente correttamente con ASP.NET 2,0 e Visual Studio 2005. Tuttavia, alcune applicazioni demo possono utilizzare funzionalità nuove per la versione .NET Framework 3,5; Quando vengono usate le funzionalità specifiche di 3,5, includo una nota in cui viene illustrato come implementare funzionalità simili nella versione 2,0. Tenere presente che le applicazioni demo disponibili per il download da ogni esercitazione sono destinate alla .NET Framework versione 3,5, che consente di ottenere un file `Web.config` che include elementi di configurazione specifici di 3,5 e riferimenti a spazi dei nomi specifici di 3,5 nelle istruzioni `using` nelle classi code-behind di ASP.NET Pages. Long Story, se è ancora necessario installare .NET 3,5 nel computer, l'applicazione Web scaricabile non funzionerà senza prima rimuovere il markup specifico di 3,5 da `Web.config`. Per ulteriori informazioni su questo argomento, vedere [dissezione del File `Web.config` della versione 3.5 di ASP.NET](http://www.4guysfromrolla.com/articles/121207-1.aspx) . Sarà inoltre necessario rimuovere le istruzioni `using` che fanno riferimento a spazi dei nomi specifici di 3,5.

## <a name="step-1-creating-a-master-page"></a>Passaggio 1: creazione di una pagina master

Prima di poter esplorare la creazione e l'uso di pagine master e di contenuto, è necessario prima di tutto un sito Web ASP.NET. Per iniziare, creare un nuovo sito Web ASP.NET basato su file system. A tale scopo, avviare Visual Web Developer, quindi scegliere nuovo sito Web dal menu file, visualizzando la finestra di dialogo nuovo sito Web (vedere la figura 4). Scegliere il modello di sito Web ASP.NET, impostare l'elenco a discesa percorso su file System, scegliere una cartella in cui collocare il sito Web e impostare il linguaggio su C#. Verrà creato un nuovo sito Web con una `Default.aspx` pagina ASP.NET, una cartella `App_Data` e un file `Web.config`.

> [!NOTE]
> Visual Studio supporta due modalità di gestione dei progetti: progetti di siti Web e progetti di applicazione Web. I progetti di siti Web non dispongono di un file di progetto, mentre i progetti di applicazioni Web simulano l'architettura del progetto in Visual Studio .NET 2002/2003, includono un file di progetto e compilano il codice sorgente del progetto in un unico assembly, che viene inserito nella cartella `/bin`. Visual Studio 2005 inizialmente solo i progetti di siti Web supportati, sebbene il [modello di progetto applicazione Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) sia stato reintrodotto con Service Pack 1. Visual Studio 2008 offre entrambi i modelli di progetto. Le edizioni Visual Web Developer 2005 e 2008, tuttavia, supportano solo i progetti di siti Web. In questa serie di esercitazioni utilizzo il modello di progetto di sito Web per le demo personali. Se invece si utilizza un'edizione non Express e si desidera utilizzare il modello di progetto di applicazione Web, è possibile farlo, ma tenere presente che potrebbero esserci alcune discrepanze tra gli elementi visualizzati sullo schermo e i passaggi da eseguire rispetto agli screenshot mostrati e Instructio NS fornito in queste esercitazioni.

[![creare un nuovo sito Web basato su file System](creating-a-site-wide-layout-using-master-pages-cs/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image8.png)

**Figura 04**: creare un nuovo sito Web basato su file System ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-site-wide-layout-using-master-pages-cs/_static/image10.png))

Successivamente, aggiungere una pagina master al sito nella directory radice facendo clic con il pulsante destro del mouse sul nome del progetto, scegliendo Aggiungi nuovo elemento e selezionando il modello di pagina master. Si noti che le pagine master terminano con l'estensione `.master`. Assegnare un nome alla nuova pagina master `Site.master` e fare clic su Aggiungi.

[![aggiungere una pagina master denominata Site. master al sito Web](creating-a-site-wide-layout-using-master-pages-cs/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image11.png)

**Figura 05**: aggiungere una pagina Master denominata `Site.master` al sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-site-wide-layout-using-master-pages-cs/_static/image13.png))

L'aggiunta di un nuovo file di pagina master tramite Visual Web Developer crea una pagina master con il markup dichiarativo seguente:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample1.aspx)]

La prima riga nel markup dichiarativo è la [direttiva`@Master`](https://msdn.microsoft.com/library/ms228176.aspx). La direttiva `@Master` è simile alla [direttiva`@Page`](https://msdn.microsoft.com/library/ydy4x04a.aspx) visualizzata nelle pagine di ASP.NET. Definisce la lingua del lato server (C#) e le informazioni sulla posizione e l'ereditarietà della classe code-behind della pagina master.

Il `DOCTYPE` e il markup dichiarativo della pagina vengono visualizzati sotto la direttiva `@Master`. La pagina include HTML statico insieme a quattro controlli lato server:

- **Un Web Form (il `<form runat="server">`)** , perché tutte le pagine di ASP.NET hanno in genere un Web Form, perché la pagina master può includere controlli Web che devono essere visualizzati all'interno di un Web Form. Assicurarsi di aggiungere il Web Form alla pagina master, anziché aggiungere un Web Form a ogni pagina di contenuto.
- **Un controllo ContentPlaceHolder denominato `ContentPlaceHolder1`** -questo controllo ContentPlaceHolder viene visualizzato all'interno del Web Form e funge da area per l'interfaccia utente della pagina di contenuto.
- **Elemento `<head>` lato server** : l'elemento `<head>` dispone dell'attributo `runat="server"`, rendendolo accessibile tramite codice sul lato server. L'elemento `<head>` viene implementato in questo modo in modo che sia possibile aggiungere o modificare il titolo della pagina e altri markup correlati a `<head>`a livello di codice. Se ad esempio si imposta la proprietà `Title` della pagina ASP.NET, viene modificato l'elemento `<title>` di cui è stato eseguito il rendering dal controllo `<head>` server.
- **Un controllo ContentPlaceHolder denominato `head`** -questo controllo ContentPlaceHolder viene visualizzato all'interno del controllo `<head>` server e può essere utilizzato per aggiungere in modo dichiarativo il contenuto all'elemento `<head>`.

Questo markup dichiarativo predefinito della pagina master funge da punto di partenza per la progettazione di pagine master personalizzate. È possibile modificare il codice HTML o aggiungere altri controlli Web o ContentPlaceHolders alla pagina master.

> [!NOTE]
> Quando si progetta una pagina master, assicurarsi che la pagina master contenga un modulo Web e che in questo Web form venga visualizzato almeno un controllo ContentPlaceHolder.

### <a name="creating-a-simple-site-layout"></a>Creazione di un layout di sito semplice

Espandere il markup dichiarativo predefinito di `Site.master`per creare un layout del sito in cui tutte le pagine condividono: un'intestazione comune; una colonna a sinistra con spostamento, notizie e altro contenuto a livello di sito; e un piè di pagina che Visualizza l'icona "con tecnologia Microsoft ASP.NET". Nella figura 6 viene illustrato il risultato finale della pagina master quando una delle pagine di contenuto viene visualizzata tramite un browser. L'area rossa del cerchio nella figura 6 è specifica della pagina visitata (`Default.aspx`); gli altri contenuti sono definiti nella pagina master e pertanto sono coerenti in tutte le pagine di contenuto.

[![pagina master definisce il markup per le parti superiore, sinistra e inferiore](creating-a-site-wide-layout-using-master-pages-cs/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image14.png)

**Figura 06**: la pagina master definisce il markup per le parti superiore, sinistra e inferiore ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-site-wide-layout-using-master-pages-cs/_static/image16.png))

Per ottenere il layout del sito illustrato nella figura 6, iniziare aggiornando la pagina master di `Site.master` in modo che contenga il markup dichiarativo seguente:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample2.aspx)]

Il layout della pagina master viene definito utilizzando una serie di elementi HTML `<div>`. Il `topContent` `<div>` contiene il markup visualizzato nella parte superiore di ogni pagina, mentre i `mainContent`, `leftContent`e `footerContent` `<div>` s vengono utilizzati per visualizzare il contenuto della pagina, la colonna sinistra e l'icona "basata su Microsoft ASP.NET", rispettivamente. Oltre ad aggiungere questi elementi di `<div>`, è stata rinominata anche la proprietà `ID` del controllo ContentPlaceHolder primario da `ContentPlaceHolder1` a `MainContent`.

Le regole di formattazione e di layout per questi elementi di `<div>` Assorted sono state digitate nel file [CSS (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) `Styles.css`, che viene specificato tramite un elemento &lt;link&gt; nell'elemento &lt;Head&gt; della pagina master. Queste varie regole definiscono l'aspetto di ogni `<div>` elemento indicato in precedenza. Ad esempio, l'elemento `topContent` `<div>`, che Visualizza il testo e il collegamento "Master Pages Tutorials", ha le regole di formattazione specificate in `Styles.css` come indicato di seguito:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample3.css)]

Se si sta seguendo il computer, sarà necessario scaricare il codice allegato di questa esercitazione e aggiungere il file di `Styles.css` al progetto. Analogamente, sarà anche necessario creare una cartella denominata images e copiare l'icona "Powered by Microsoft ASP.NET" dal sito Web demo scaricato al progetto.

> [!NOTE]
> Una discussione sulla formattazione di CSS e pagine Web esula dall'ambito di questo articolo. Per ulteriori informazioni su CSS, vedere le [esercitazioni CSS](http://www.w3schools.com/css/default.asp) in [w3schools.com](http://www.w3schools.com/). Si consiglia inoltre di scaricare il codice allegato di questa esercitazione e di provare le impostazioni CSS in `Styles.css` per visualizzare gli effetti delle diverse regole di formattazione.

### <a name="creating-a-master-page-using-an-existing-design-template"></a>Creazione di una pagina master utilizzando un modello di progettazione esistente

Nel corso degli anni ho creato un certo numero di applicazioni Web ASP.NET per le aziende di piccole e medie dimensioni. Alcuni client hanno un layout del sito esistente che volevano usare. altri hanno assunto una finestra di progettazione grafica competente. Alcuni si sono affidati alla progettazione del layout del sito Web. Come illustrato nella figura 6, l'attività di un programmatore di progettare il layout di un sito Web è in genere altrettanto saggia quanto la necessità di un proprio contabile di eseguire interventi a cuore aperto, mentre il medico esegue le imposte.

Fortunatamente, sono disponibili numerosi siti Web che offrono modelli di progettazione HTML gratuiti: Google ha restituito più di 6 milioni risultati per il termine di ricerca "modelli di siti web gratuiti". Uno dei miei Preferiti è [OpenDesigns.org](http://opendesigns.org/). Una volta trovato un modello di sito Web, aggiungere i file e le immagini CSS al progetto del sito Web e integrare il codice HTML del modello nella pagina master.

> [!NOTE]
> Microsoft offre inoltre una serie di [modelli gratuiti di avvio della progettazione di ASP.NET](https://msdn.microsoft.com/asp.net/aa336613.aspx) che si integrano nella finestra di dialogo nuovo sito Web in Visual Studio.

## <a name="step-2-creating-associated-content-pages"></a>Passaggio 2: creazione di pagine di contenuto associate

Con la pagina master creata, è possibile iniziare a creare pagine ASP.NET associate alla pagina master. Tali pagine sono denominate *pagine di contenuto*.

Aggiungere una nuova pagina ASP.NET al progetto e associarla alla pagina master `Site.master`. Fare clic con il pulsante destro del mouse sul nome del progetto in Esplora soluzioni e scegliere l'opzione Aggiungi nuovo elemento. Selezionare il modello Web Form, immettere il nome `About.aspx`, quindi selezionare la casella di controllo "Seleziona pagina master", come illustrato nella figura 7. In questo modo viene visualizzata la finestra di dialogo Seleziona una pagina master (vedere la figura 8) dalla quale è possibile scegliere la pagina master da usare.

> [!NOTE]
> Se il sito Web ASP.NET è stato creato usando il modello di progetto di applicazione Web anziché il modello di progetto di sito Web, nella finestra di dialogo Aggiungi nuovo elemento non sarà visualizzata la casella di controllo "Seleziona pagina master" nella figura 7. Per creare una pagina contenuto quando si utilizza il modello di progetto applicazione Web, è necessario scegliere il modello di modulo contenuto Web anziché il modello Web Form. Dopo aver selezionato il modello di form contenuto Web e aver fatto clic su Aggiungi, verrà visualizzata la stessa finestra di dialogo Seleziona una pagina master mostrata nella figura 8.

[![aggiungere una nuova pagina contenuto](creating-a-site-wide-layout-using-master-pages-cs/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image17.png)

**Figura 07**: aggiungere una nuova pagina di contenuto ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-site-wide-layout-using-master-pages-cs/_static/image19.png))

[![selezionare la pagina master del sito. master](creating-a-site-wide-layout-using-master-pages-cs/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image20.png)

**Figura 08**: selezionare la pagina Master di `Site.master` ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-site-wide-layout-using-master-pages-cs/_static/image22.png))

Come illustrato nel markup dichiarativo seguente, una nuova pagina contenuto contiene una direttiva `@Page` che fa riferimento alla pagina master e a un controllo contenuto per ognuno dei controlli ContentPlaceHolder della pagina master.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample4.aspx)]

> [!NOTE]
> Nella sezione "creazione di un layout di sito semplice" nel passaggio 1 ho rinominato `ContentPlaceHolder1` `MainContent`. Se la `ID` del controllo ContentPlaceHolder non è stata rinominata nello stesso modo, il markup dichiarativo della pagina contenuto sarà leggermente diverso dal markup riportato sopra. In particolare, il secondo `ContentPlaceHolderID` del controllo contenuto rifletterà l'`ID` del controllo ContentPlaceHolder corrispondente nella pagina master.

Quando si esegue il rendering di una pagina di contenuto, il motore ASP.NET deve fondere i controlli contenuto della pagina con i controlli ContentPlaceHolder della pagina master. Il motore ASP.NET determina la pagina master della pagina contenuto dall'attributo `MasterPageFile` della direttiva `@Page`. Come illustrato nel markup precedente, la pagina di contenuto è associata a `~/Site.master`.

Poiché la pagina master contiene due controlli ContentPlaceHolder: `head` e `MainContent`-Visual Web Developer ha generato due controlli contenuto. Ogni controllo contenuto fa riferimento a un ContentPlaceHolder specifico tramite la relativa proprietà `ContentPlaceHolderID`.

Le pagine master che risplendono alle tecniche dei modelli a livello di sito precedenti sono basate sul supporto della fase di progettazione. Nella figura 9 è illustrata la pagina di contenuto `About.aspx` se visualizzata tramite la visualizzazione progettazione di Visual Web Developer. Si noti che mentre il contenuto della pagina master è visibile, è disabilitato e non può essere modificato. I controlli contenuto corrispondenti ai ContentPlaceHolders della pagina master sono, tuttavia, modificabili. Analogamente a qualsiasi altra pagina di ASP.NET, è possibile creare l'interfaccia della pagina di contenuto aggiungendo controlli Web tramite le visualizzazioni di origine o progettazione.

[![visualizzazione progettazione della pagina contenuto Visualizza sia il contenuto della pagina master che quello specifico della pagina](creating-a-site-wide-layout-using-master-pages-cs/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image23.png)

**Figura 09**: la visualizzazione progettazione della pagina contenuto Visualizza sia il contenuto della pagina master che quello specifico della pagina ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-site-wide-layout-using-master-pages-cs/_static/image25.png))

### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Aggiunta di markup e controlli Web alla pagina contenuto

Creazione di contenuto per la pagina `About.aspx`. Come si può vedere nella figura 10, ho immesso un'intestazione "about the author" (informazioni sull'autore) e un paio di paragrafi di testo, ma anche di aggiungere I controlli Web. Dopo aver creato questa interfaccia, visitare la pagina `About.aspx` tramite un browser.

[![visitare la pagina about. aspx tramite un browser](creating-a-site-wide-layout-using-master-pages-cs/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image26.png)

**Figura 10**: visitare la pagina `About.aspx` tramite un browser ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-site-wide-layout-using-master-pages-cs/_static/image28.png))

È importante comprendere che la pagina di contenuto richiesta e la pagina master associata vengono fuse e sottoposte a rendering nel suo complesso interamente sul server Web. Al browser dell'utente finale viene quindi inviato il codice HTML risultante, con fuso. Per verificarlo, visualizzare il codice HTML ricevuto dal browser scegliendo origine dal menu Visualizza. Si noti che non sono presenti frame o altre tecniche specializzate per la visualizzazione di due pagine Web diverse in un'unica finestra.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Associazione di una pagina master a una pagina ASP.NET esistente

Come illustrato in questo passaggio, l'aggiunta di una nuova pagina di contenuto a un'applicazione Web ASP.NET è semplice quanto selezionare la casella di controllo "Seleziona pagina master" e scegliere la pagina master. Sfortunatamente, la conversione di una pagina ASP.NET esistente in una pagina master non è altrettanto semplice.

Per associare una pagina master a una pagina ASP.NET esistente, è necessario eseguire i passaggi seguenti:

1. Aggiungere l'attributo `MasterPageFile` alla direttiva `@Page` della pagina ASP.NET, facendola puntare alla pagina master appropriata.
2. Aggiungere i controlli contenuto per ogni ContentPlaceHolders nella pagina master.
3. Taglia e incolla selettivamente il contenuto esistente della pagina ASP.NET nei controlli contenuto appropriati. Dico "selettivamente" perché la pagina ASP.NET contiene probabilmente il markup già espresso dalla pagina master, ad esempio il `DOCTYPE`, l'elemento `<html>` e il Web Form.

Per istruzioni dettagliate su questo processo insieme a schermate, vedere l'esercitazione sull' [uso di pagine master e navigazione nel sito](http://webproject.scottgu.com/CSharp/MasterPages/MasterPages.aspx) di [Scott Guthrie](https://weblogs.asp.net/scottgu/). La sezione "aggiornare `Default.aspx` e `DataSample.aspx` per usare la pagina master" illustra i passaggi seguenti.

Poiché è molto più semplice creare nuove pagine di contenuto rispetto alla conversione di pagine ASP.NET esistenti in pagine di contenuto, è consigliabile che ogni volta che si crea un nuovo sito Web di ASP.NET aggiungere una pagina master al sito. Associa tutte le nuove pagine di ASP.NET a questa pagina master. Non preoccuparti se la pagina master iniziale è molto semplice o semplice; è possibile aggiornare la pagina master in un secondo momento.

> [!NOTE]
> Quando si crea una nuova applicazione ASP.NET, Visual Web Developer aggiunge una pagina `Default.aspx` che non è associata a una pagina master. Se si vuole provare a convertire una pagina di ASP.NET esistente in una pagina di contenuto, procedere con `Default.aspx`. In alternativa, è possibile eliminare `Default.aspx` e quindi aggiungerlo di nuovo, ma questa volta selezionare la casella di controllo "Seleziona pagina master".

## <a name="step-3-updating-the-master-pages-markup"></a>Passaggio 3: aggiornamento del markup della pagina master

Uno dei principali vantaggi delle pagine master è che è possibile utilizzare una singola pagina master per definire il layout generale per numerose pagine del sito. Pertanto, l'aspetto dell'aggiornamento del sito richiede l'aggiornamento di un singolo file, ovvero la pagina master.

Per illustrare questo comportamento, è necessario aggiornare la pagina master per includere la data corrente nella parte superiore della colonna sinistra. Aggiungere un'etichetta denominata `DateDisplay` al `<div>``leftContent`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample5.aspx)]

Successivamente, creare un gestore dell'evento `Page_Load` per la pagina master e aggiungere il codice seguente:

[!code-csharp[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample6.cs)]

Il codice precedente imposta la proprietà `Text` dell'etichetta sulla data e l'ora correnti formattate come il giorno della settimana, il nome del mese e il giorno a due cifre (vedere la figura 11). Con questa modifica, visitare nuovamente una delle pagine di contenuto. Come illustrato nella figura 11, il markup risultante viene immediatamente aggiornato per includere la modifica nella pagina master.

[![le modifiche apportate alla pagina master vengono riflesse durante la visualizzazione della pagina di contenuto](creating-a-site-wide-layout-using-master-pages-cs/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image29.png)

**Figura 11**: le modifiche apportate alla pagina master vengono riflesse durante la visualizzazione della pagina di contenuto ([fare clic per visualizzare l'immagine con dimensioni complete](creating-a-site-wide-layout-using-master-pages-cs/_static/image31.png))

> [!NOTE]
> Come illustrato in questo esempio, le pagine master possono contenere controlli Web lato server, codice e gestori eventi.

## <a name="summary"></a>Riepilogo

Le pagine master consentono agli sviluppatori ASP.NET di progettare un layout coerente a livello di sito facilmente aggiornabile. La creazione di pagine master e delle relative pagine di contenuto associate è semplice come la creazione di pagine ASP.NET standard, in quanto Visual Web Developer offre un supporto avanzato in fase di progettazione.

Nell'esempio di pagina master creato in questa esercitazione sono presenti due controlli ContentPlaceHolder, `head` e `MainContent`. Tuttavia, nella pagina di contenuto è stato specificato solo il markup per il controllo `MainContent` ContentPlaceHolder. Nell'esercitazione successiva si osserverà l'uso di più controlli contenuto nella pagina contenuto. Viene inoltre illustrato come definire il markup predefinito per i controlli contenuto all'interno della pagina master, nonché come passare dall'utilizzo del markup predefinito definito nella pagina master e fornire markup personalizzato dalla pagina contenuto.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [ASP.NET per le finestre di progettazione: modelli di progettazione gratuiti e linee guida per la creazione di siti Web ASP.NET con standard Web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Cenni preliminari sulle pagine master ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Esercitazioni sui fogli di stile CSS](http://www.w3schools.com/css/default.asp)
- [Impostazione dinamica del titolo della pagina](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Pagine master in ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Esercitazioni introduttive sulle pagine master](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 3,5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott può essere raggiunto in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [avanti](multiple-contentplaceholders-and-default-content-cs.md)
