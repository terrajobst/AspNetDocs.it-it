---
uid: whitepapers/mvc3-release-notes
title: MVC ASP.NET 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/06/2010
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 46d051a5eba6501cf36910b7674ce6400597de8a
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057025"
---
# <a name="aspnet-mvc-3"></a>ASP.NET MVC 3

- [Panoramica](#overview)
- [Note sull'installazione](#installation-notes)
- [Requisiti software](#software-requirements)
- [Documentazione](#documentation)
- [Supporto](#support)
- [Aggiornamento di un progetto ASP.NET MVC 2 a ASP.NET MVC 3 Tools Update](#upgrading)
- [Aggiornamento degli strumenti di ASP.NET MVC 3 (12 aprile 2011)](#tu-changes)

    - [La finestra di dialogo "Aggiungi controller" può ora usare i controller di ponteggi con visualizzazioni e codice di accesso ai dati](#tu-AddControllerDialog)
    - [Miglioramenti alla finestra di dialogo "ASP.NET MVC 3 nuovo progetto"](#tu-ImprovementsNewDialogBox)
    - [I modelli di progetto includono ora modernizzatore 1,7](#tu-Modernizr)
    - [I modelli di progetto includono le versioni aggiornate di jQuery, jQuery UI e jQuery Validation](#tu-UpdatedJQuery)
    - [I modelli di progetto includono ora ADO.NET Entity Framework 4,1 come pacchetto NuGet preinstallato](#tu-EF)
    - [I modelli di progetto includono le librerie JavaScript come pacchetti NuGet preinstallati](#tu-JavaScriptLibsNuget)
    - [Problemi noti](#tu-KI)
- [ASP.NET MVC 3 RTM (13 gennaio 2011)](#MVC3RTM)

    - [Modifica: aggiornamento della versione dell'interfaccia utente di jQuery a 1.8.7](#RTM-1)
    - [Modifica: è stato modificato il valore predefinito di ModelMetadataProvider in DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Corretto: la parte di un'espressione Razor che contiene spazi vuoti risulta invertita](#RTM-3)
    - [Problema risolto: la ridenominazione di un file Razor aperto nell'editor Disabilita la colorazione della sintassi e IntelliSense](#RTM-4)
    - [Problemi noti](#RTM-KI)
    - [Modifiche di rilievo](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (10 dicembre 2010)](#_Toc2)

    - [I modelli di progetto sono stati modificati in modo da includere jQuery 1.4.4, jQuery Validation 1,7 e jQuery UI 1.8.6 y UI 1.8.6](#_Toc2_1)
    - [Aggiunta classe "AdditionalMetadataAttribute"](#_Toc2_2)
    - [Miglioramento dell'impalcatura delle visualizzazioni](#_Toc2_3)
    - [Aggiunto metodo HTML. Raw](#_Toc2_3)
    - [La proprietà "controller. ViewModel" e la proprietà "View" sono state rinominate in "ViewBag"](#_Toc2_4)
    - [La classe "ControllerSessionStateAttribute" è stata rinominata in "SessionStateAttribute"](#_Toc2_5)
    - [La proprietà "Fields" di RemoteAttribute è stata rinominata in "AdditionalFields"](#_Toc2_6)
    - [Rinominato "SkipRequestValidationAttribute" in "AllowHtmlAttribute"](#_Toc2_7)
    - [È stato modificato il metodo "HTML. ValidationMessage" per visualizzare il primo messaggio di errore utile](#_Toc2_8)
    - [Correzione della dichiarazione di @model per non aggiungere spazi vuoti al documento](#_Toc2_9)
    - [Aggiunta proprietà "FileExtensions" per visualizzare i motori per supportare i nomi file specifici del motore](#_Toc2_10)
    - [Correzione dell'helper "LabelFor" per creare il valore corretto per l'attributo "for"](#_Toc2_11)
    - [Correzione del metodo "RenderAction" per assegnare la precedenza ai valori espliciti durante l'associazione di modelli](#_Toc2_12)
    - [Modifiche di rilievo](#_Toc2_BC)
    - [Problemi noti](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (9 novembre, 2010)](#TOC_ASP_NET_3_RC)

    - [Nuove funzionalità di ASP.NET MVC 3 RC](#_Toc276711785)
    - [Gestione pacchetti NuGet](#_Toc276711786)
    - [Finestra di dialogo "nuovo progetto" migliorata](#_Toc276711787)
    - [Controller non con sessione](#_Toc276711788)
    - [Nuovi attributi di convalida](#_Toc276711789)
    - [Nuovi overload per i metodi "LabelFor" e "LabelForModel"](#_Toc276711790)
    - [Memorizzazione nella cache dell'output dell'azione figlio](#_Toc276711791)
    - [Miglioramenti alla finestra di dialogo "Aggiungi visualizzazione"](#_Toc276711792)
    - [Convalida di richieste granulari](#_Toc276711793)
    - [Modifiche di rilievo](#_Toc276711794)
    - [Problemi noti](#_Toc276711795)
- [ASP. Note su MVC 3 beta (6 ottobre 2010)](#TOC_ASP_NET_3_Beta)

    - [Nuove funzionalità di ASP.NET MVC 3 beta](#0.1__Toc274034215)
    - [Gestione pacchetti NuPack](#0.1__Toc274034216)
    - [Finestra di dialogo nuovo progetto migliorata](#0.1__Toc274034217)
    - [Modo semplificato per specificare modelli fortemente tipizzati nelle visualizzazioni Razor](#0.1__Toc274034218)
    - [Supporto per nuovi metodi helper Pagine Web ASP.NET](#0.1__Toc274034219)
    - [Ulteriore supporto per l'inserimento di dipendenze](#0.1__Toc274034220)
    - [Nuovo supporto per AJAX non intrusivo basato su jQuery](#0.1__Toc274034221)
    - [Nuovo supporto per la convalida di jQuery non intrusiva](#0.1__Toc274034222)
    - [Nuovi flag a livello di applicazione per la convalida del client e JavaScript non intrusivo](#0.1__Toc274034223)
    - [Nuovo supporto per il codice eseguito prima dell'esecuzione delle visualizzazioni](#0.1__Toc274034224)
    - [Nuovo supporto per la sintassi Razor di VBHTML](#0.1__Toc274034225)
    - [Controllo più granulare su ValidateInputAttribute](#0.1__Toc274034226)
    - [Gli helper convertono i caratteri di sottolineatura in trattini per i nomi di attributi HTML specificati usando oggetti anonimi](#0.1__Toc274034227)
    - [Correzioni di bug](#0.1__Toc274034228)
    - [Modifiche di rilievo](#0.1__Toc274034229)
    - [Problemi noti](#0.1__Toc274034230)
- [Non responsabilità](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Panoramica

Questo documento descrive la versione di ASP.NET MVC 3 RTM per Visual Studio 2010. ASP.NET MVC è un Framework per lo sviluppo di applicazioni Web che usano il modello MVC (Model-View-Controller). Il programma di installazione di ASP.NET MVC 3 include i componenti seguenti:

- Componenti di runtime di ASP.NET MVC 3
- Strumenti di Visual Studio 2010 per ASP.NET MVC 3
- Componenti della fase di esecuzione Pagine Web ASP.NET
- Strumenti di Pagine Web ASP.NET Visual Studio 2010
- Microsoft Package Manager per .NET (NuGet)
- Aggiornamento di Visual Studio 2010 che Abilita il supporto per sintassi Razor. Per informazioni dettagliate, vedere l'articolo della Knowledge base 2483190.

Il set completo di note sulla versione per ogni versione preliminare di ASP.NET MVC 3 è reperibile nel sito Web ASP.NET all'URL seguente:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Note sull'installazione

Per installare ASP.NET MVC 3 RTM usando l'installazione guidata piattaforma Web (PI Web), visitare la pagina seguente:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

In alternativa, è possibile scaricare il programma di installazione per ASP.NET MVC 3 RTM per Visual Studio 2010 dalla pagina seguente:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 può essere installato e può essere eseguito side-by-side con ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Requisiti software

I componenti della fase di esecuzione di ASP.NET MVC 3 richiedono il software seguente:

- .NET Framework versione 4. 

    Gli strumenti di Visual Studio 2010 per ASP.NET MVC 3 richiedono il software seguente:
- Visual Studio 2010 o Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

La documentazione relativa a ASP.NET MVC è disponibile nel sito Web MSDN all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Le esercitazioni e altre informazioni su ASP.NET MVC sono disponibili nella pagina MVC del sito Web ASP.NET all'URL seguente:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Supporto

Si tratta di una versione completamente supportata. Informazioni su come ottenere supporto tecnico sono disponibili nel [sito web supporto tecnico Microsoft](https://support.microsoft.com/).

È anche possibile pubblicare domande su questa versione nel forum di ASP.NET MVC, in cui i membri della community di ASP.NET sono spesso in grado di fornire supporto informale:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Aggiornamento di un progetto ASP.NET MVC 2 a ASP.NET MVC 3 Tools Update

ASP.NET MVC 3 può essere installato side-by-side con ASP.NET MVC 2 nello stesso computer, che offre la flessibilità necessaria per scegliere quando aggiornare un'applicazione ASP.NET MVC 2 a ASP.NET MVC 3.

Per aggiornare manualmente un'applicazione ASP.NET MVC 2 esistente alla versione 3, seguire questa procedura:

1. Creare un nuovo progetto ASP.NET MVC 3 vuoto nel computer. Questo progetto conterrà alcuni file necessari per l'aggiornamento.
2. Copiare i file seguenti dal progetto ASP.NET MVC 3 nel percorso corrispondente del progetto ASP.NET MVC 2. È necessario aggiornare tutti i riferimenti alla libreria jQuery per tenere conto del nuovo nome file (jQuery-1.5.1. js): 

    - /Views/Web.config
    - /packages.config
    - /script/\*. js
    - \*/content/themes/.\*
3. Copiare la cartella *packages* nella radice della soluzione del progetto ASP.NET MVC 3 vuota nella radice della soluzione, che si trova nella directory in cui si trova il file con estensione sln della soluzione.
4. Se il progetto ASP.NET MVC 2 contiene aree, copiare il file/Views/Web.config nella cartella *views* di ogni area.
5. In entrambi i file Web. config del progetto ASP.NET MVC 2 eseguire una ricerca globale e sostituire la versione ASP.NET MVC. Trovare gli elementi seguenti: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Sostituirlo con quanto segue:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. In Esplora soluzioni eliminare il riferimento a *System. Web. Mvc* (che punta alla DLL della versione 2), quindi aggiungere un riferimento a *System. Web. Mvc* (v 3.0.0.0).
7. Aggiungere un riferimento a System. Web. WebPages. dll e System. Web. Helpers. dll. Questi assembly si trovano nelle cartelle seguenti: 

    - % Programmi% \ Microsoft ASP. NET\ASP.NET MVC 3 \ assembly
    - % Programmi% \ Microsoft ASP. NET\ASP.NET Web Pages\v1.0\Assemblies
8. In Esplora soluzioni fare clic con il pulsante destro del mouse sul nome del progetto e scegliere Scarica progetto. Quindi fare di nuovo clic con il pulsante destro del mouse sul nome del progetto e scegliere modifica *NomeProgetto*. csproj.
9. Individuare l'elemento *ProjectTypeGuids* e sostituire {F85E285D-a4e0-4152-9332-AB1D724D3325} con {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Salvare le modifiche, fare clic con il pulsante destro del mouse sul progetto e quindi scegliere Ricarica progetto.
11. Nel file Web. config radice dell'applicazione aggiungere le impostazioni seguenti alla sezione *assembly* . 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Se il progetto fa riferimento a qualsiasi libreria di terze parti compilata con ASP.NET MVC 2, aggiungere l'elemento *bindingRedirect* evidenziato seguente al file Web. config nella radice dell'applicazione nella sezione di *configurazione* : 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Modifiche apportate a ASP.NET MVC 3 Tools Update

Questa sezione descrive le modifiche apportate alla versione di aggiornamento degli strumenti di ASP.NET MVC 3 dalla versione ASP.NET MVC 3 RTM.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>La finestra di dialogo "Aggiungi controller" può ora usare i controller di ponteggi con visualizzazioni e codice di accesso ai dati

L'impalcatura è un modo per generare rapidamente un controller e le visualizzazioni per l'applicazione. Dopo la generazione del codice, è possibile modificarlo per soddisfare i requisiti del progetto.

Per aprire la finestra di dialogo *Aggiungi controller* in ASP.NET MVC 3, fare clic con il pulsante destro del mouse sulla cartella *controllers* in *Esplora soluzioni*, scegliere *Aggiungi*e quindi fare clic su *controller*. La finestra di dialogo è stata migliorata per offrire opzioni aggiuntive per l'impalcatura.

![](mvc3-release-notes/_static/image1.png)

Sono disponibili tre modelli di impalcatura per impostazione predefinita.

#### <a name="empty-controller"></a>Controller vuoto

Questo modello genera un file controller vuoto. Questo modello equivale a non selezionare *Aggiungi azioni per creare, modificare, dettagli, eliminare scenari* nelle versioni precedenti di ASP.NET MVC. Se si sceglie questa opzione, non sono disponibili altre opzioni.

#### <a name="controller-with-empty-readwrite-actions"></a>Controller con azioni di lettura/scrittura vuote

Questo modello genera un file controller che contiene tutti i metodi di azione necessari, ma nessun codice di implementazione nei metodi. Questo modello equivale a selezionare *Aggiungi azioni per creare, modificare, dettagli, eliminare scenari* nelle versioni precedenti di ASP.NET MVC. Se si sceglie questa opzione, non sono disponibili altre opzioni.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Controller con azioni di lettura/scrittura e visualizzazioni, usando Entity Framework

Questo modello consente di creare rapidamente un'interfaccia utente di immissione dati funzionante. Genera codice che gestisce un intervallo di requisiti e scenari comuni, come i seguenti:

- *Accesso ai dati*. Il codice generato legge e scrive le entità in un database. Funziona con l'approccio Entity Framework Code First se si sceglie una classe del contesto dati esistente o se si consente al modello di generare una nuova classe *DbContext* . Funziona anche con l'approccio Entity Framework Database First o Model First se si sceglie una classe *ObjectContext* esistente.
- *Convalida*. Il codice generato usa le funzionalità di associazione di modelli e metadati di ASP.NET MVC, in modo che gli invii di moduli vengano convalidati in base alle regole dichiarate nella classe del modello. Sono incluse le regole di convalida predefinite, ad esempio gli attributi *required* e *StringLength* e le regole di convalida personalizzate.
- *Relazioni uno-a-molti*. Se si definiscono relazioni di chiave esterna uno-a-molti tra le classi del modello, il codice generato produrrà elenchi a discesa per la selezione delle entità correlate. È possibile, ad esempio, definire le classi del modello seguenti Entity Framework convenzioni di Code First: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Quando si esegue il patibolo di un controller per la classe *Product* , le visualizzazioni consentiranno agli utenti di scegliere un oggetto *Category* per ogni istanza del *prodotto* .

    Questo modello Abilita opzioni aggiuntive nella finestra di dialogo *Aggiungi controller* . Per la *classe Model*è possibile scegliere qualsiasi classe di modello nella soluzione, che determina il tipo di dati che gli utenti saranno in grado di creare o modificare:
- Se si desidera utilizzare Entity Framework Code First, è possibile scegliere qualsiasi classe di modello.
- Se si usa Entity Framework Database First o Entity Framework Model First, assicurarsi di scegliere una classe di entità definita nel modello concettuale.

Per la *classe del contesto dati*è possibile effettuare le scelte seguenti:

- Se si desidera utilizzare Code First e non si dispone di alcuna classe del contesto dati esistente, scegliere * * nuovo contesto dati * *. Una classe del contesto dati verrà quindi generata automaticamente.
- Se si vuole usare Code First e avere una classe del contesto dati esistente, selezionarla qui. Verrà aggiornata per salvare in modo permanente la classe del modello selezionata.
- Se si usa Database First o Model First, scegliere la classe del contesto dell'oggetto qui.

Per le visualizzazioni, scegliere il motore di visualizzazione che si vuole usare oppure scegliere nessuno se non si vuole creare un impalcatura per le visualizzazioni.

È possibile selezionare OPTIONSper avanzati per specificare altre opzioni per le visualizzazioni generate. Ad esempio, è possibile scegliere il layout o la pagina master da utilizzare.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Miglioramenti alla finestra di dialogo "ASP.NET MVC 3 nuovo progetto"

La finestra di dialogo utilizzata per creare nuovi progetti ASP.NET MVC 3 include vari miglioramenti, come indicato di seguito.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nuovo modello "progetto Intranet"

L'elenco dei modelli di progetto include un nuovo modello di applicazione Intranet. Questo modello contiene le impostazioni per la compilazione di un'applicazione Web utilizzando l'autenticazione di Windows anziché l'autenticazione basata su form. Poiché un'applicazione Intranet richiede alcune impostazioni IIS che non possono essere incapsulate in un modello di progetto, il modello include un file Leggimi con le istruzioni su come fare in modo che il modello di progetto funzioni in IIS. La documentazione per il nuovo modello di applicazione Intranet è disponibile nel sito Web MSDN all'URL seguente:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>I modelli di progetto sono ora abilitati per HTML5

La finestra di dialogo nuovo progetto contiene ora un'opzione che consente di aggiungere funzionalità specifiche di HTML5 ai modelli di progetto. Se si seleziona l'opzione, le visualizzazioni vengono generate che contengono i nuovi elementi HTML5 `<header>`, `<footer>`e `<navigation>`.

Si noti che le versioni precedenti dei browser non supportano i tag specifici di HTML5. Per risolvere questa limitazione, i modelli di progetto HTML5 includono un riferimento alla libreria Modernizzator. Vedere la sezione successiva.

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>I modelli di progetto includono ora modernizzatore 1,7

Modernizzator è una libreria JavaScript che Abilita il supporto per CSS 3 e HTML5 nei browser che non supportano ancora queste funzionalità. Questa libreria è inclusa come pacchetto NuGet preinstallato nei modelli per i progetti ASP.NET MVC 3. Per ulteriori informazioni su Modernizzator, vedere [http://www.modernizr.com/](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>I modelli di progetto includono le versioni aggiornate di jQuery, jQuery UI e jQuery Validation

I modelli di progetto includono ora le versioni seguenti degli script jQuery:

- jQuery 1.5.1
- Convalida di jQuery 1,8
- interfaccia utente di jQuery 1.8.11

Queste librerie sono incluse come pacchetti NuGet preinstallati.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>I modelli di progetto includono ora ADO.NET Entity Framework 4,1 come pacchetto NuGet preinstallato

Il Entity Framework 4,1 di ADO.NET include la funzionalità Code First. Code First è un nuovo modello di sviluppo per il Entity Framework ADO.NET che fornisce un'alternativa ai modelli di Database First e Model First esistenti.

Code First si concentra sulla definizione del modello utilizzando le classi POCO ("Plain Old CLR Objects") scritte in C#Visual Basic o. È quindi possibile eseguire il mapping di queste classi a un database esistente o utilizzarle per generare uno schema di database. È possibile specificare una configurazione aggiuntiva usando gli attributi *DataAnnotations* o le API Fluent.

La documentazione per l'uso del codice Firstwith ASP.NET MVC è disponibile nel sito Web ASP.NET agli URL seguenti:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>I modelli di progetto includono le librerie JavaScript come pacchetti NuGet preinstallati

Quando si crea un nuovo progetto ASP.NET MVC 3, il progetto include i file JavaScript indicati in precedenza (ad esempio, la libreria di modernizzatore) tramite l'installazione di NuGet invece di aggiungere direttamente gli script alla cartella Scripts nel modello di progetto contenuto. In questo modo è possibile usare NuGet per aggiornare gli script alla versione più recente quando vengono rilasciate nuove versioni degli script.

Ad esempio, data la frequenza delle nuove versioni di jQuery, la versione di jQuery inclusa nel modello di progetto non sarà aggiornata. Tuttavia, poiché jQuery è incluso come pacchetto NuGet installato, si riceverà una notifica nella finestra di dialogo NuGet quando saranno disponibili versioni più recenti di jQuery.

Poiché jQuery include il numero di versione nel nome del file, l'aggiornamento di jQuery alla versione più recente richiede anche l'aggiornamento del tag `<script>` che fa riferimento al file jQuery per usare il nuovo nome file. Le altre librerie di script incluse non includono il numero di versione nel nome dello script, quindi possono essere aggiornate più facilmente alle versioni più recenti.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Problemi noti

- In alcuni casi, l'installazione potrebbe non riuscire con il messaggio di errore "installazione non riuscita con codice errore (0x80070643)". Per informazioni su come risolvere questo problema, vedere l' [articolo della Knowledge base 2531566](https://support.microsoft.com/kb/2531566).
- L'impalcatura per l'aggiunta di un controller non esegue l'impalcatura delle entità che sfruttano il supporto per l'ereditarietà delle entità all'interno Entity Framework. Ad esempio, data una classe *Person* di base ereditata da una classe *Student* , l'impalcatura della classe *Student* comporterà il codice generato che non viene compilato.
- La creazione di un nuovo progetto ASP.NET MVC 3 all'interno di una cartella della soluzione genera un errore *NullReferenceException* . La soluzione alternativa consiste nel creare il progetto ASP.NET MVC 3 nella radice della soluzione e quindi spostarlo nella cartella della soluzione.
- IntelliSense per sintassi Razor non funziona quando è installato resharper. Se è stato installato resharper e si vuole sfruttare i vantaggi del supporto IntelliSense per Razor in ASP.NET MVC 3, vedere la voce [Razor IntelliSense and resharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) nel Blog di Hadi Hariri, che illustra i modi per usarli insieme oggi stesso.
- Durante l'installazione, la finestra di dialogo accettazione EULA Visualizza le condizioni di licenza in una finestra di dimensioni inferiori a quelle previste.
- Quando si modifica una visualizzazione Razor (. cshtml o. *file vbhtml* ), viste. ASP.NET MVC 3 non include frammenti di codice per le visualizzazioni Razor. aspxselecting un frammento di codice per ASP.NET MVC visualizzerà i frammenti per
- Se si installa ASP.NET MVC 3 per Visual Web Developer Express in un computer in cui non è installato Visual Studio e successivamente si installa Visual Studio, è necessario reinstallare ASP.NET MVC 3. Visual Studio e Visual Web Developer Express condividono componenti che vengono aggiornati dal programma di installazione di ASP.NET MVC 3. Lo stesso problema si verifica se si installa ASP.NET MVC 3 per Visual Studio in un computer in cui non è installato Visual Web Developer Express e successivamente si installa Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Modifiche apportate a ASP.NET MVC 3 RTM

Questa sezione descrive le modifiche e le correzioni di bug apportate nella versione ASP.NET MVC 3 RTM dalla versione RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Modifica: aggiornamento della versione dell'interfaccia utente di jQuery a 1.8.7

I modelli di progetto MVC ASP.NET per Visual Studio sono stati aggiornati in modo da includere la versione più recente della libreria dell'interfaccia utente di jQuery. I modelli includono anche il set minimo di file di risorse richiesti dall'interfaccia utente di jQuery, ad esempio i file di immagine e CSS associati.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Modifica: è stato modificato il valore predefinito di ModelMetadataProvider in DataAnnotationsModelMetadataProvider

La versione RC2 di ASP.NET MVC 3 ha introdotto una classe *CachedDataAnnotationsMetadataProvider* che ha fornito la memorizzazione nella cache sulla classe *DataAnnotationsModelMetadataProvider* esistente come miglioramento delle prestazioni. Tuttavia, alcuni bug sono stati segnalati con questa implementazione, quindi la modifica è stata ripristinata e spostata nel progetto MVC Futures, disponibile in [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Corretto: la parte di un'espressione Razor che contiene spazi vuoti risulta invertita

Nelle versioni non definitive di ASP.NET MVC 3, quando si incolla una parte di un'espressione Razor che contiene spazi vuoti in un file Razor, l'espressione risultante viene invertita. Si consideri, ad esempio, il blocco di codice Razor seguente:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Se si seleziona il testo "First param" nel primo metodo e lo si incolla come argomento nel secondo metodo, il risultato sarà il seguente:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Il comportamento corretto consiste nel fare in modo che l'operazione Incolla provochi gli elementi seguenti:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Questo problema è stato risolto nella versione RTM, in modo che l'espressione venga mantenuta correttamente durante l'operazione Incolla.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Problema risolto: la ridenominazione di un file Razor aperto nell'editor Disabilita la colorazione della sintassi e IntelliSense

Se si rinomina un file Razor usando Esplora soluzioni mentre il file viene aperto nella finestra dell'editor, l'evidenziazione della sintassi e IntelliSense smetteranno di funzionare per quel file. Questo problema è stato risolto in modo che l'evidenziazione e IntelliSense vengano mantenuti dopo una ridenominazione.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Problemi noti

- Se si chiude Visual Studio 2010 SP1 Beta mentre la console di gestione pacchetti NuGet è aperta, Visual Studio si arresta in modo anomalo e tenta di riavviare. Questo problema verrà risolto nella versione RTM di Visual Studio 2010 SP1.
- Il programma di installazione di ASP.NET MVC 3 è in grado di installare solo una versione iniziale di gestione pacchetti NuGet. Dopo aver installato la versione iniziale, NuGet può essere installato e aggiornato con gestione estensioni di Visual Studio. Se NuGet è già installato, passare alla raccolta di estensioni di Visual Studio per eseguire l'aggiornamento alla versione più recente di NuGet.
- La creazione di un nuovo progetto ASP.NET MVC 3 all'interno di una cartella della soluzione genera un errore *NullReferenceException* . La soluzione alternativa consiste nel creare il progetto ASP.NET MVC 3 nella radice della soluzione e quindi spostarlo nella cartella della soluzione.
- Il completamento del programma di installazione potrebbe richiedere molto più tempo rispetto alle versioni precedenti di ASP.NET MVC. Questo perché aggiorna i componenti di Visual Studio 2010.
- IntelliSense per sintassi Razor non funziona quando è installato resharper. Se è stato installato resharper e si vuole sfruttare i vantaggi del supporto IntelliSense per Razor in ASP.NET MVC 3, vedere la voce [Razor IntelliSense and resharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) nel Blog di Hadi Hariri, che illustra i modi per usarli insieme oggi stesso.
- Le visualizzazioni CCSHTML e VBHTML create con la versione beta di ASP.NET MVC 3 non hanno un set di azioni di compilazione corretto, con il risultato che questi tipi di visualizzazione vengono omessi al momento della pubblicazione del progetto. Il valore dell'azione di compilazione per questi file deve essere impostato su "Content". ASP.NET MVC 3 RTM corregge questo problema per i nuovi file, ma non corregge l'impostazione per i file esistenti per un progetto creato con versioni provvisorie.
- ![](mvc3-release-notes/_static/image3.png)
- Durante l'installazione, la finestra di dialogo accettazione EULA Visualizza le condizioni di licenza in una finestra di dimensioni inferiori a quelle previste.
- Quando si modifica una visualizzazione Razor (file con estensione cshtml), la voce di menu Vai al controller in Visual Studio non sarà disponibile e non ci sono frammenti di codice.
- Se si installa ASP.NET MVC 3 per Visual Web Developer Express in un computer in cui non è installato Visual Studio e successivamente si installa Visual Studio, è necessario reinstallare ASP.NET MVC 3. Visual Studio e Visual Web Developer Express condividono componenti che vengono aggiornati dal programma di installazione di ASP.NET MVC 3. Lo stesso problema si verifica se si installa ASP.NET MVC 3 per Visual Studio in un computer in cui non è installato Visual Web Developer Express e successivamente si installa Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Modifiche di interruzione

- Nelle versioni precedenti di ASP.NET MVC, i filtri azione sono creati per ogni richiesta tranne che in alcuni casi. Questo comportamento non è mai un comportamento garantito, ma solo un dettaglio di implementazione e il contratto per i filtri li consideravano senza stato. In ASP.NET MVC 3, i filtri vengono memorizzati nella cache in modo più aggressivo. Pertanto, qualsiasi filtro azioni personalizzato che archivia in modo errato lo stato dell'istanza potrebbe essere danneggiato.
- L'ordine di esecuzione dei filtri eccezioni è stato modificato per i filtri eccezioni con lo stesso valore di *ordine* . In ASP.NET MVC 2 e versioni precedenti, i filtri delle eccezioni sul controller che hanno lo stesso valore di *ordine* di quelli su un metodo di azione vengono eseguiti prima dei filtri delle eccezioni nel metodo di azione. Questa situazione si verifica in genere quando vengono applicati filtri eccezioni senza un valore di *ordine* specificato. In ASP.NET MVC 3 questo ordine è stato invertito in modo che il gestore di eccezioni più specifico venga eseguito per primo. Come nelle versioni precedenti, se la proprietà *Order* viene specificata in modo esplicito, i filtri vengono eseguiti nell'ordine specificato.
- È stata aggiunta una nuova proprietà denominata *FileExtension* alla classe di base *VirtualPathProviderViewEngine* . Quando ASP.NET cerca una vista in base al percorso (non in base al nome), vengono considerate solo le viste con un'estensione di file contenuta nell'elenco specificato da questa nuova proprietà. Si tratta di una modifica sostanziale nelle applicazioni in cui viene registrato un provider di compilazione personalizzato per abilitare un'estensione di file personalizzata per le visualizzazioni Web Form e il provider a cui fa riferimento tali viste utilizzando un percorso completo anziché un nome. La soluzione alternativa consiste nel modificare il valore della proprietà *FileExtensions* in modo da includere l'estensione di file personalizzata.
- Le implementazioni personalizzate della factory del controller che implementano direttamente l'interfaccia *IControllerFactory* devono fornire un'implementazione del nuovo metodo *GetControllerSessionBehavior* aggiunto all'interfaccia in questa versione. In generale, è consigliabile non implementare direttamente questa interfaccia e derivare la classe da *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Modifiche apportate a ASP.NET MVC 3 RC2

Questa sezione descrive le modifiche (nuove funzionalità e correzioni di bug) apportate nella versione ASP.NET MVC 3 RC2 dalla versione RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>I modelli di progetto sono stati modificati in modo da includere jQuery 1.4.4, jQuery Validation 1,7 e jQuery UI 1.8.6

I modelli di progetto per ASP.NET MVC 3 includono ora le versioni più recenti di jQuery, la convalida jQuery e l'interfaccia utente di jQuery. jQuery UI è una nuova aggiunta ai modelli di progetto e fornisce widget di interfaccia utente utili. Per ulteriori informazioni sull'interfaccia utente di jQuery, visitare la Home page: [http://jqueryui.com/](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Aggiunta classe "AdditionalMetadataAttribute"

È possibile utilizzare la classe *AdditionalMetadataAttribute* per popolare il dizionario *ModelMetadata. AdditionalValues* per una proprietà del modello.

Si supponga ad esempio che un modello di visualizzazione disponga di proprietà che devono essere visualizzate solo da un amministratore. Tale modello può essere annotato con il nuovo attributo usando AdminOnly come chiave e true come valore, come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Questi metadati vengono resi disponibili per qualsiasi modello di visualizzazione o di Editor quando viene eseguito il rendering di un modello di visualizzazione del prodotto. Gli sviluppatori di applicazioni devono interpretare le informazioni sui metadati.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Miglioramento dell'impalcatura delle visualizzazioni

I modelli T4 usati per le visualizzazioni di ponteggi generano ora chiamate a metodi helper del modello, ad esempio *EditorFor* anziché helper, ad esempio *TextBoxFor*. Questa modifica migliora il supporto dei metadati nel modello sotto forma di attributi di annotazione dei dati quando la finestra di dialogo Aggiungi visualizzazione genera una visualizzazione.

L'impalcatura per l'aggiunta di viste include anche un miglioramento del rilevamento e dell'utilizzo delle informazioni sulla chiave primaria sul modello, in base alla convenzione. La finestra di dialogo Aggiungi visualizzazione, ad esempio, utilizza queste informazioni per garantire che il valore della chiave primaria non sia sottoporre a impalcatura come campo modulo modificabile.

I modelli predefiniti di modifica e creazione includono riferimenti agli script jQuery necessari per la convalida del client.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Aggiunto metodo HTML. Raw

Per impostazione predefinita, il motore di visualizzazione Razor codifica in HTML tutti i valori. Ad esempio, il frammento di codice seguente codifica il codice HTML all'interno della variabile di saluto, in modo che venga visualizzato nella pagina come `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Il nuovo metodo *HTML. Raw* fornisce un modo semplice per visualizzare il codice HTML non codificato quando il contenuto è noto come sicuro. Nell'esempio seguente viene visualizzata la stessa stringa, ma viene eseguito il rendering della stringa come markup:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>La proprietà "controller. ViewModel" e la proprietà "View" sono state rinominate in "ViewBag"

In precedenza, la proprietà *ViewModel* del *controller* corrispondeva alla proprietà *View* di una vista. Entrambe queste proprietà consentono di accedere ai valori dell'oggetto *ViewDataDictionary* utilizzando la sintassi dinamica della funzione di accesso alle proprietà. Entrambe le proprietà sono state rinominate in modo che siano uguali per evitare confusione e essere più coerenti.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>La classe "ControllerSessionStateAttribute" è stata rinominata in "SessionStateAttribute"

La classe *ControllerSessionStateAttribute* è stata introdotta nella versione RC di ASP.NET MVC 3. La proprietà è stata rinominata in modo da essere più concisa.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>La proprietà "Fields" di RemoteAttribute è stata rinominata in "AdditionalFields"

La proprietà *Fields* della classe *RemoteAttribute* ha causato confusione tra gli utenti. La ridenominazione di questa proprietà in *AdditionalFields* ne consente di chiarirne lo scopo.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Rinominato "SkipRequestValidationAttribute" in "AllowHtmlAttribute"

L'attributo *SkipRequestValidationAttribute* è stato rinominato in *AllowHtmlAttribute* per rappresentare meglio l'utilizzo previsto.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>È stato modificato il metodo "HTML. ValidationMessage" per visualizzare il primo messaggio di errore utile

Il metodo *HTML. ValidationMessage* è stato corretto per mostrare il primo messaggio di errore utile anziché visualizzare semplicemente il primo errore.

Durante l'associazione di modelli, il dizionario *ModelState* può essere popolato da più origini con messaggi di errore relativi alla proprietà, inclusi dal modello stesso (se implementa *IValidatableObject*), dagli attributi di convalida applicati alla proprietà e dalle eccezioni generate durante l'accesso alla proprietà.

Quando il metodo *HTML. ValidationMessage* Visualizza un messaggio di convalida, ignora le voci di stato del modello che includono un'eccezione, perché in genere non sono destinate all'utente finale. Il metodo cerca invece il primo messaggio di convalida che non è associato a un'eccezione e visualizza il messaggio. Se non viene trovato alcun messaggio di questo tipo, per impostazione predefinita viene utilizzato un messaggio di errore generico associato alla prima eccezione.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Correzione della dichiarazione di @model per non aggiungere spazi vuoti al documento

Nelle versioni precedenti, la dichiarazione `@model` nella parte superiore di una vista ha aggiunto una riga vuota all'output HTML sottoposto a rendering. Questo problema è stato risolto in modo che la dichiarazione non introduca spazi vuoti.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Aggiunta proprietà "FileExtensions" per visualizzare i motori per supportare i nomi file specifici del motore

Un motore di visualizzazione può restituire una visualizzazione usando un percorso di visualizzazione esplicito come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

Il primo motore di visualizzazione tenta sempre di eseguire il rendering della visualizzazione. Per impostazione predefinita, il motore di visualizzazione Web Form è il primo motore di visualizzazione. Poiché il motore Web Form non è in grado di eseguire il rendering di una visualizzazione Razor, si verifica un errore. I motori di visualizzazione dispongono ora di una proprietà *FileExtensions* che consente di specificare le estensioni di file supportate. Questa proprietà viene verificata quando ASP.NET determina se un motore di visualizzazione è in grado di eseguire il rendering di un file. Si tratta di una modifica di rilievo e altri dettagli sono inclusi nella sezione [modifiche di rilievo](#_Toc2_BC) di questo documento.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Correzione dell'helper "LabelFor" per creare il valore corretto per l'attributo "for"

È stato corretto un bug in cui il metodo *LabelFor* ha eseguito il rendering di un oggetto *per* l'attributo che corrisponde all'attributo *Name* dell'elemento di *input* anziché al relativo ID. In base a W3C, l'attributo *for* deve corrispondere all'ID dell'elemento di *input* .

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Correzione del metodo "RenderAction" per assegnare la precedenza ai valori espliciti durante l'associazione di modelli

Nelle versioni precedenti, i valori espliciti passati al metodo *RenderAction* venivano ignorati a favore dei valori di form correnti durante l'associazione del modello all'interno di un'azione figlio. La correzione garantisce che i valori espliciti abbiano la precedenza durante l'associazione del modello.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Modifiche di interruzione

- Nelle versioni precedenti di ASP.NET MVC, i filtri azione sono stati creati per ogni richiesta tranne che in alcuni casi. Questo comportamento non è mai un comportamento garantito, ma solo un dettaglio di implementazione e il contratto per i filtri li consideravano senza stato. In ASP.NET MVC 3, i filtri vengono memorizzati nella cache in modo più aggressivo. Pertanto, qualsiasi filtro azioni personalizzato che archivia in modo errato lo stato dell'istanza potrebbe essere danneggiato.
- L'ordine di esecuzione dei filtri eccezioni è stato modificato per i filtri eccezioni con lo stesso valore di *ordine* . In ASP.NET MVC 2 e versioni precedenti, i filtri delle eccezioni sul controller con lo stesso valore di *ordine* di quelli su un metodo di azione venivano eseguiti prima dei filtri delle eccezioni nel metodo di azione. Questa situazione si verifica in genere quando vengono applicati filtri eccezioni senza un valore di *ordine* specificato. In ASP.NET MVC 3 questo ordine è stato invertito in modo che il gestore di eccezioni più specifico venga eseguito per primo. Come nelle versioni precedenti, se la proprietà *Order* viene specificata in modo esplicito, i filtri vengono eseguiti nell'ordine specificato.
- È stata aggiunta una nuova proprietà denominata *FileExtension* alla classe di base *VirtualPathProviderViewEngine* . Quando ASP.NET cerca una vista in base al percorso (non in base al nome), vengono considerate solo le viste con un'estensione di file contenuta nell'elenco specificato da questa nuova proprietà. Si tratta di una modifica sostanziale nelle applicazioni in cui viene registrato un provider di compilazione personalizzato per abilitare un'estensione di file personalizzata per le visualizzazioni Web Form e il provider a cui fa riferimento tali viste utilizzando un percorso completo anziché un nome. La soluzione alternativa consiste nel modificare il valore della proprietà *FileExtensions* in modo da includere l'estensione di file personalizzata.
- Le implementazioni personalizzate della factory del controller che implementano direttamente l'interfaccia *IControllerFactory* devono fornire un'implementazione del nuovo metodo *GetControllerSessionBehavior* aggiunto all'interfaccia in questa versione. In generale, è consigliabile non implementare direttamente questa interfaccia e derivare la classe da *DefaultControllerFactory*.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Problemi noti

- Il programma di installazione di ASP.NET MVC 3 è in grado di installare solo una versione iniziale di gestione pacchetti NuGet. Dopo aver installato la versione iniziale, NuGet può essere installato e aggiornato con gestione estensioni di Visual Studio. Se NuGet è già installato, passare alla raccolta di estensioni di Visual Studio per eseguire l'aggiornamento alla versione più recente di NuGet.
- La creazione di un nuovo progetto ASP.NET MVC 3 all'interno di una cartella della soluzione genera un errore *NullReferenceException* . La soluzione alternativa consiste nel creare il progetto ASP.NET MVC 3 nella radice della soluzione e quindi spostarlo nella cartella della soluzione.
- Il completamento del programma di installazione potrebbe richiedere molto più tempo rispetto alle versioni precedenti di ASP.NET MVC. Questo perché aggiorna i componenti di Visual Studio 2010.
- IntelliSense per sintassi Razor non funziona quando è installato resharper. Se è stato installato resharper e si vuole sfruttare i vantaggi del supporto IntelliSense per Razor in ASP.NET MVC 3 RC2, vedere la voce [Razor IntelliSense and resharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) nel Blog di Hadi Hariri, che illustra i modi per usarli insieme oggi stesso.
- Le visualizzazioni CSHTML e VBHTML create con la versione beta di ASP.NET MVC 3 non hanno un set di azioni di compilazione corretto, con il risultato che questi tipi di visualizzazione vengono omessi al momento della pubblicazione del progetto. Il valore dell' *azione di compilazione* per questi file deve essere impostato su Content ". ASP.NET MVC 3 RC2 corregge questo problema per i nuovi file, ma non corregge l'impostazione per i file esistenti per un progetto creato con la versione beta.![](mvc3-release-notes/_static/image4.png)
- Durante l'installazione, la finestra di dialogo accettazione EULA Visualizza le condizioni di licenza in una finestra di dimensioni inferiori a quelle previste.
- Quando si modifica una visualizzazione Razor (file con estensione cshtml), la voce di menu Vai al controller in Visual Studio non sarà disponibile e non ci sono frammenti di codice.
- Se si installa ASP.NET MVC 3 per Visual Web Developer Express in un computer in cui non è installato Visual Studio e successivamente si installa Visual Studio, è necessario reinstallare ASP.NET MVC 3. Visual Studio e Visual Web Developer Express condividono componenti che vengono aggiornati dal programma di installazione di ASP.NET MVC 3. Lo stesso problema si verifica se si installa ASP.NET MVC 3 per Visual Studio in un computer in cui non è installato Visual Web Developer Express e successivamente si installa Visual Web Developer Express.
- L'installazione di ASP.NET MVC 3 RC 2 non aggiorna NuGet se è già installata. Per aggiornare NuGet, passare a gestione estensioni di Visual Studio e dovrebbe essere visualizzato come aggiornamento disponibile. È possibile aggiornare NuGet alla versione più recente da qui.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 Release Candidate

La versione finale candidata di ASP.NET MVC è stata rilasciata il 9 novembre 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nuove funzionalità di ASP.NET MVC 3 RC

Questa sezione descrive le funzionalità introdotte nella versione ASP.NET MVC 3 RC dalla versione beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Gestione pacchetti NuGet

ASP.NET MVC 3 include gestione pacchetti NuGet (precedentemente noto come NuPack), uno strumento di gestione dei pacchetti integrato per l'aggiunta di librerie e strumenti ai progetti di Visual Studio. Questo strumento consente di automatizzare i passaggi eseguiti attualmente dagli sviluppatori per ottenere una libreria nell'albero di origine.

È possibile usare NuGet come strumento da riga di comando, come finestra della console integrata all'interno di Visual Studio 2010, dal menu di scelta rapida di Visual Studio e come un set di cmdlet di PowerShell.

Per ulteriori informazioni su NuGet, vedere la [documentazione di NuGet](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Finestra di dialogo "nuovo progetto" migliorata

Quando si crea un nuovo progetto, nella finestra di dialogo nuovo progetto è ora possibile specificare il motore di visualizzazione e un tipo di progetto MVC ASP.NET.

![](mvc3-release-notes/_static/image5.png)

In questa versione è incluso il supporto per la modifica dell'elenco di modelli e dei motori di visualizzazione elencati nella finestra di dialogo.

I modelli predefiniti sono i seguenti:

Vuoto. Contiene un set minimo di file per un progetto MVC ASP.NET, inclusa la struttura di directory predefinita per i progetti ASP.NET MVC, un file site. CSS che contiene gli stili di MVC ASP.NET predefiniti e una directory di script che contiene i file JavaScript predefiniti.

Applicazione Internet. Contiene la funzionalità di esempio che illustra come usare il provider di appartenenze con ASP.NET MVC.

L'elenco dei modelli di progetto visualizzato nella finestra di dialogo viene specificato nel registro di sistema di Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Controller non con sessione

Il nuovo *ControllerSessionStateAttribute* offre un maggiore controllo sul comportamento dello stato della sessione per i controller specificando un valore di enumerazione [System. Web. SessionState. SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) .

Nell'esempio seguente viene illustrato come disattivare lo stato della sessione per tutte le richieste a un controller.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Nell'esempio seguente viene illustrato come impostare lo stato della sessione di sola lettura per tutte le richieste a un controller.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nuovi attributi di convalida

#### <a name="compareattribute"></a>CompareAttribute

Il nuovo attributo di convalida *CompareAttribute* consente di confrontare i valori di due proprietà diverse di un modello. Nell'esempio seguente, la proprietà *ComparePassword* deve corrispondere al campo *password* per essere valida.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Il nuovo attributo di convalida *RemoteAttribute* sfrutta il validator remoto del plug-in di convalida jQuery, che consente alla convalida sul lato client di chiamare un metodo sul server che esegue la logica di convalida effettiva.

Nell'esempio seguente, alla proprietà *username* è applicato *RemoteAttribute* . Quando si modifica questa proprietà in una visualizzazione di modifica, la convalida del client chiamerà un'azione denominata *UserNameAvailable* sulla classe *UsersController* per convalidare questo campo.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Nell'esempio seguente viene illustrato il controller corrispondente.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Per impostazione predefinita, il nome della proprietà a cui viene applicato l'attributo viene inviato al metodo di azione come parametro della stringa di query.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nuovi overload per i metodi "LabelFor" e "LabelForModel"

Sono stati aggiunti nuovi overload per i metodi *LabelFor* e *LabelForModel* che consentono di specificare il testo dell'etichetta. Nell'esempio seguente viene illustrato come utilizzare questi overload.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Memorizzazione nella cache dell'output dell'azione figlio

*OutputCacheAttribute* supporta la memorizzazione nella cache dell'output delle azioni figlio chiamate usando i metodi helper *HTML. RenderAction* o *HTML. Action* . Nell'esempio seguente viene illustrata una vista che chiama un'altra azione.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

L'azione *GETDATE* è annotata con *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Quando viene eseguito questo codice, il risultato della chiamata a HTML. Action ("GETDATE") viene memorizzato nella cache per 100 secondi.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Miglioramenti alla finestra di dialogo "Aggiungi visualizzazione"

Quando si aggiunge una visualizzazione fortemente tipizzata, nella finestra di dialogo Aggiungi visualizzazione vengono ora filtrati più tipi non applicabili rispetto alle versioni precedenti, ad esempio molti tipi di .NET Framework di base. Inoltre, l'elenco è ora ordinato in base al nome della classe e non in base al nome completo del tipo, semplificando la ricerca dei tipi. Ad esempio, il nome del tipo viene ora visualizzato come nell'esempio seguente:

NomeClasse (spazio dei nomi)

Nelle versioni precedenti, questo sarebbe stato visualizzato come segue:

Namespace. NomeClasse

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Convalida di richieste granulari

La proprietà *Exclude* di *ValidateInputAttribute* non esiste più. Per fare in modo che la convalida delle richieste venga ignorata per proprietà specifiche di un modello durante l'associazione di modelli, usare il nuovo *SkipRequestValidationAttribute*.

Si supponga, ad esempio, che venga usato un metodo di azione per modificare un post di Blog:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Nell'esempio seguente viene illustrato il modello di visualizzazione per un post di Blog.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Quando un utente invia un markup per la proprietà Description, l'associazione di modelli non riuscirà a causa della convalida della richiesta. Per disabilitare la convalida delle richieste durante l'associazione di modelli per la descrizione del post di Blog, applicare *SkipRequpestValidationAttribute* alla proprietà, come illustrato nell'esempio seguente:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

In alternativa, per disattivare la convalida delle richieste per ogni proprietà del modello, applicare *ValidateInputAttribute* con il valore *false* al metodo di azione:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Modifiche di interruzione

- L'ordine di esecuzione dei filtri eccezioni è stato modificato per i filtri eccezioni con lo stesso valore di *ordine* . In ASP.NET MVC 2 e versioni precedenti, i filtri delle eccezioni sul controller con lo stesso *ordine* di quelli su un metodo di azione venivano eseguiti prima dei filtri delle eccezioni nel metodo di azione. Questa situazione si verifica in genere quando vengono applicati filtri eccezioni senza un valore di *ordine* specificato. In ASP.NET MVC 3 questo ordine è stato invertito in modo che il gestore di eccezioni più specifico venga eseguito per primo. Come nelle versioni precedenti, se la proprietà *Order* viene specificata in modo esplicito, i filtri vengono eseguiti nell'ordine specificato.
- Aggiunta di una nuova proprietà denominata *FileExtension* alla classe di base *VirtualPathProviderViewEngine* . Quando si cerca una vista in base al percorso (e non in base al nome), vengono considerate solo le viste con un'estensione di file contenuta nell'elenco specificato da questa nuova proprietà. Si tratta di una modifica di rilievo per coloro che registrano un provider di compilazione personalizzato per abilitare un'estensione di file personalizzata per le visualizzazioni Web Form e che fanno riferimento a tali viste utilizzando un percorso completo anziché un nome. La soluzione alternativa consiste nel modificare il valore della proprietà *FileExtensions* in modo da includere l'estensione di file personalizzata.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Problemi noti

- Il programma di installazione potrebbe richiedere molto più tempo rispetto alle versioni precedenti di ASP.NET MVC perché aggiorna i componenti di Visual Studio 2010.
- Aggiunta dell'impalcatura della visualizzazione quando si selezionano le proprietà di sola scrittura delle impalcature della visualizzazione tipizzata astrongly. Questi devono essere sempre ignorati dall'impalcatura. La finestra di dialogo Aggiungi visualizzazione esegue anche l'impalcatura delle proprietà di sola lettura durante la generazione di una visualizzazione "modifica" o "Crea". Le proprietà di sola lettura devono essere sottoposte a impalcatura solo per le visualizzazioni visualizzazione ed elenco.
- Il debug non funziona quando si installa ASP.NET MVC 3 insieme alla versione CTP asincrona. ASP.NET MVC 3 non può essere installato side-by-side con la versione CTP asincrona. Disinstallare la versione CTP asincrona per ripristinare il debug. Per informazioni dettagliate, leggere [questo post di Blog](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) sulla disinstallazione di tutte le parti di ASP.NET MVC 3 RC.
- Razor IntelliSense non funziona quando è installato resharper. Se è stato installato resharper e si vuole sfruttare i vantaggi del supporto IntelliSense per Razor in ASP.NET MVC 3 RC, leggere [questo post di Blog](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) di JetBrains che illustra i modi per utilizzarli insieme.
- Le visualizzazioni CSHTML e VBHTML create con beta di ASP.NET MVC 3 non hanno un'azione di compilazione corretta che le omette dalla pubblicazione. L' *azione di compilazione* per questi file deve essere impostata su "Content". ASP.NET MVC 3 RC corregge questo problema per i nuovi file, ma non corregge l'impostazione per i file esistenti per un progetto creato con la versione beta.
- Il programma di installazione potrebbe richiedere molto più tempo rispetto alle versioni precedenti di ASP.NET MVC perché aggiorna i componenti di Visual Studio 2010.
- Aggiunta dell'impalcatura della visualizzazione quando si selezionano le proprietà di sola lettura "modifica" delle impalcature di visualizzazione fortemente tipizzata. Analogamente, le proprietà di sola scrittura sono basate su impalcature per le visualizzazioni "visualizzate".
- Durante l'installazione, la finestra di dialogo accettazione EULA Visualizza le condizioni di licenza in una finestra di dimensioni inferiori a quelle previste.
- L'installazione di Visual Studio Async CTP causa un conflitto con la versione Razor inclusa come parte dell'installazione degli strumenti di ASP.NET MVC 3. Assicurarsi di non provare a installare sia Visual Studio Async CTP che la versione Razor nello stesso computer.
- Quando si modifica una visualizzazione Razor (file con estensione cshtml), la voce di menu Vai al controller in Visual Studio non sarà disponibile e non ci sono frammenti di codice.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 beta

ASP.NET MVC 3 beta è stato rilasciato il 6 ottobre 2010. Le note seguenti sono specifiche della versione beta e sono soggette a eventuali aggiornamenti o modifiche a cui viene fatto riferimento nella sezione ASP.NET MVC 3 Release Candidate riportata sopra.

## <a id="0.1__Toc274034215"></a>Nuove funzionalità di ASP.NET MVC 3 beta

<a id="0.1__Default_validation_system"></a>Questa sezione descrive le funzionalità introdotte nella versione beta di ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>Gestione pacchetti NuGet

ASP.NET MVC 3 include gestione pacchetti NuGet, uno strumento integrato per la gestione dei pacchetti per l'aggiunta di librerie e strumenti ai progetti di Visual Studio. Nella maggior parte dei casi, consente di automatizzare i passaggi che gli sviluppatori intraprendono oggi per ottenere una libreria nell'albero di origine.

È possibile usare NuGet come strumento da riga di comando, come finestra della console integrata all'interno di Visual Studio 2010, dal menu di scelta rapida di Visual Studio e come set di cmdlet di PowerShell.

Per ulteriori informazioni su NuGet, vedere la [documentazione di NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>Finestra di dialogo nuovo progetto migliorata

Quando si crea un nuovo progetto, nella finestra di dialogo nuovo progetto è ora possibile specificare il motore di visualizzazione e un tipo di progetto MVC ASP.NET.

![](mvc3-release-notes/_static/image6.png)

Il supporto per la modifica dell'elenco di modelli e dei motori di visualizzazione elencati nella finestra di dialogo non è incluso in questa versione.

I modelli predefiniti sono i seguenti:

Vuoto. Contiene un set minimo di file per un progetto MVC ASP.NET, inclusa la struttura di directory predefinita per i progetti ASP.NET MVC, un piccolo file site. CSS che contiene gli stili di MVC ASP.NET predefiniti e una directory scripts che contiene i file JavaScript predefiniti.

Applicazione Internet. Contiene la funzionalità di esempio che illustra come usare il provider di appartenenze in ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>Modo semplificato per specificare modelli fortemente tipizzati nelle visualizzazioni Razor

Il modo in cui è possibile specificare il tipo di modello per le visualizzazioni Razor fortemente tipizzate è stato semplificato usando la nuova direttiva @model per le visualizzazioni CSHTML e la direttiva @ModelType per le visualizzazioni VBHTML. Nelle versioni precedenti di ASP.NET MVC, è necessario specificare un modello fortemente tipizzato per le visualizzazioni Razor in questo modo:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

In questa versione, è possibile usare la sintassi seguente:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>Supporto per nuovi metodi helper Pagine Web ASP.NET

La nuova tecnologia Pagine Web ASP.NET include un set di metodi helper utili per l'aggiunta di funzionalità comunemente utilizzate a viste e controller. ASP.NET MVC 3 supporta l'uso di questi metodi helper all'interno di controller e visualizzazioni (dove appropriato). Questi metodi sono contenuti nell'assembly System. Web. helper. Nella tabella seguente sono elencati alcuni dei metodi di supporto Pagine Web ASP.NET.

| **Helper** | **Descrizione** |
| --- | --- |
| Grafico | Esegue il rendering di un grafico all'interno di una visualizzazione. Contiene metodi quali Chart. ToWebImage, Chart. Save e Chart. Write. |
| Crypto | USA gli algoritmi di hash per creare password con salting e hash corrette. |
| WebGrid | Esegue il rendering di una raccolta di oggetti, in genere dati da un database, come griglia. Supporta il paging e l'ordinamento. |
| WebImage | Esegue il rendering di un'immagine. |
| WebMail | Invia un messaggio di posta elettronica. |

Un argomento di riferimento rapido in cui sono elencati gli helper e la sintassi di base è disponibile come parte della documentazione di ASP.NET sintassi Razor all'URL seguente:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>Ulteriore supporto per l'inserimento di dipendenze

Basandosi sulla versione di ASP.NET MVC 3 Preview 1, la versione corrente include l'aggiunta del supporto per due nuovi servizi e quattro servizi esistenti, oltre al supporto migliorato per la risoluzione delle dipendenze e per Common Service Locator.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nuova interfaccia IControllerActivator per la creazione di un'istanza di controller con granularità fine

La nuova interfaccia IControllerActivator fornisce un controllo più granulare sulla modalità di creazione di istanze dei controller tramite l'inserimento di dipendenze. Nell'esempio seguente viene illustrata l'interfaccia:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

A differenza del ruolo della factory del controller. Una factory del controller è un'implementazione dell'interfaccia IControllerFactory, che è responsabile sia per l'individuazione di un tipo di controller che per la creazione di un'istanza di tale tipo di controller.

Gli attivatori del controller sono responsabili solo della creazione di un'istanza di un tipo di controller. Non eseguono la ricerca del tipo di controller. Dopo aver individuato un tipo di controller appropriato, le factory del controller dovrebbero delegare a un'istanza di IControllerActivator per gestire la creazione di istanza effettiva del controller.

La classe DefaultControllerFactory dispone di un nuovo costruttore che accetta un'istanza di IControllerFactory. Ciò consente di applicare l'inserimento delle dipendenze per gestire questo aspetto della creazione del controller senza dover eseguire l'override del comportamento di ricerca del tipo controller predefinito.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interfaccia IServiceLocator sostituita con IDependencyResolver

In base al feedback della community, la versione beta di ASP.NET MVC 3 ha sostituito l'uso dell'interfaccia IServiceLocator con un'interfaccia IDependencyResolver snella, specifica per le esigenze di ASP.NET MVC. Nell'esempio seguente viene illustrata la nuova interfaccia:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Come parte di questa modifica, la classe ServiceLocator è stata sostituita anche con la classe DependencyResolver. La registrazione di un sistema di risoluzione delle dipendenze è simile alle versioni precedenti di ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Le implementazioni di questa interfaccia devono semplicemente delegare al contenitore di inserimento delle dipendenze sottostante per fornire il servizio registrato per il tipo richiesto.

Quando non sono presenti servizi registrati del tipo richiesto, MVC ASP.NET prevede che le implementazioni di questa interfaccia restituiscano null da GetService e restituiscono una raccolta vuota da GetServices.

La nuova classe DependencyResolver consente di registrare le classi che implementano la nuova interfaccia IDependencyResolver o Common Service Locator Interface (IServiceLocator). Per altre informazioni su Common Service Locator, vedere [CommonServiceLocator su GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nuova interfaccia IViewActivator per la creazione di un'istanza di pagina di visualizzazione con granularità fine

La nuova interfaccia IViewPageActivator fornisce un controllo più granulare sul modo in cui viene creata un'istanza delle pagine di visualizzazione tramite l'inserimento di dipendenze. Questo vale sia per le istanze di WebFormView sia per le istanze RazorView. Nell'esempio seguente viene illustrata la nuova interfaccia:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Queste classi accettano ora un argomento del costruttore IViewPageActivator, che consente di usare l'inserimento di dipendenze per controllare la modalità di creazione di istanze dei tipi ViewPage, ViewUserControl e WebViewPage.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Nuovo supporto del resolver di dipendenza per i servizi esistenti

La nuova versione include il supporto per la risoluzione delle dipendenze per i servizi seguenti:

- Provider di convalida del modello. Le classi che implementano ModelValidatorProvider possono essere registrate nel sistema di risoluzione delle dipendenze e verranno utilizzate dal sistema per supportare la convalida lato client e lato server.
- Provider di metadati del modello. Una singola classe che implementa ModelMetadataProvider può essere registrata nel sistema di risoluzione delle dipendenze e verrà utilizzata dal sistema per fornire i metadati per i sistemi di creazione dei modelli e di convalida.
- Provider di valori. Le classi che implementano ValueProviderFactory possono essere registrate nel sistema di risoluzione delle dipendenze e verranno utilizzate dal sistema per creare provider di valori che vengono utilizzati dal controller e durante l'associazione di modelli.
- Associazioni di modelli. Le classi che implementano IModelBinderProvider possono essere registrate nel sistema di risoluzione delle dipendenze e verranno utilizzate dal sistema per creare gli associazione di modelli utilizzati dal sistema di associazione di modelli.

### <a id="0.1__Toc274034221"></a>Nuovo supporto per AJAX non intrusivo basato su jQuery

ASP.NET MVC include metodi helper Ajax come i seguenti:

- Ajax. ActionLink
- Ajax. RouteLink
- Ajax. BeginForm
- Ajax. BeginRouteForm

Questi metodi usano JavaScript per richiamare un metodo di azione sul server invece di usare un postback completo. Questa funzionalità è stata aggiornata per sfruttare i vantaggi di jQuery in modo discreto. Anziché emettere intrusivamente script client inline, questi metodi helper separano il comportamento dal markup creando attributi HTML5 usando il prefisso *Data-Ajax* . Il comportamento viene quindi applicato al markup facendo riferimento ai file JavaScript appropriati. Assicurarsi che venga fatto riferimento ai file JavaScript seguenti:

- jQuery-1.4.1. js
- jQuery. unobtrusive. Ajax. js

Questa funzionalità è abilitata per impostazione predefinita nel file Web. config nei nuovi modelli di progetto ASP.NET MVC 3, ma è disabilitata per impostazione predefinita per i progetti esistenti. Per ulteriori informazioni, vedere [aggiunta di flag a livello di applicazione per la convalida del client e JavaScript non intrusivo](#0.1_AddedApplicationWideFlagsForClientValida) più avanti in questo documento.

### <a id="0.1__Toc274034222"></a>Nuovo supporto per la convalida di jQuery non intrusiva

Per impostazione predefinita, ASP.NET MVC 3 beta usa la convalida jQuery in modo non intrusivo per eseguire la convalida lato client. Per abilitare la convalida client non intrusiva, effettuare una chiamata come la seguente dall'interno di una visualizzazione:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

A tale scopo, è necessario che la Proprietà ViewContext. UnobtrusiveJavaScriptEnabled sia impostata su true, eseguendo la chiamata seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Assicurarsi inoltre che venga fatto riferimento ai file JavaScript seguenti.

- jQuery-1.4.1. js
- jQuery. Validate. js
- jQuery. Validate. unintrusive. js

Questa funzionalità è abilitata per impostazione predefinita nel file Web. config nei nuovi modelli di progetto ASP.NET MVC 3, ma è disabilitata per impostazione predefinita per i progetti esistenti. Per ulteriori informazioni, vedere [i nuovi flag a livello di applicazione per la convalida del client e JavaScript non intrusivo](#0.1_AddedApplicationWideFlagsForClientValida) più avanti in questo documento.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>Nuovi flag a livello di applicazione per la convalida del client e JavaScript non intrusivo

È possibile abilitare o disabilitare la convalida del client e JavaScript non intrusivo a livello globale usando i membri statici della classe HtmlHelper, come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Per impostazione predefinita, i modelli di progetto predefiniti abilitano JavaScript discreto. È anche possibile abilitare o disabilitare queste funzionalità nel file Web. config radice dell'applicazione usando le impostazioni seguenti:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Poiché è possibile abilitare queste funzionalità per impostazione predefinita, sono stati introdotti nuovi overload alla classe HtmlHelper che consentono di eseguire l'override delle impostazioni predefinite, come illustrato negli esempi seguenti:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Per compatibilità con le versioni precedenti, entrambe le funzionalità sono disabilitate per impostazione predefinita.

### <a id="0.1__Toc274034224"></a>Nuovo supporto per il codice eseguito prima dell'esecuzione delle visualizzazioni

È ora possibile inserire un file denominato \_viewStart. cshtml (o \_viewStart. vbhtml) nella directory views e aggiungervi codice che verrà condiviso tra più visualizzazioni nella directory e nelle relative sottodirectory. Ad esempio, è possibile inserire il codice seguente nella pagina \_viewStart. cshtml nella cartella ~/Views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Questa impostazione consente di impostare la pagina layout per ogni visualizzazione all'interno della cartella Views e di tutte le relative sottocartelle in modo ricorsivo. Quando viene eseguito il rendering di una visualizzazione, il codice nel \_file viewStart. cshtml viene eseguito prima dell'esecuzione del codice di visualizzazione. Il codice \_viewStart. cshtml si applica a tutte le visualizzazioni della cartella.

Per impostazione predefinita, il codice nel file \_viewStart. cshtml si applica anche alle viste in qualsiasi sottocartella. Tuttavia, le singole sottocartelle possono avere una propria versione del file \_viewStart. cshtml. in tal caso, la versione locale avrà la precedenza. Ad esempio, per eseguire codice comune a tutte le visualizzazioni per HomeController, inserire un file \_viewStart. cshtml nella cartella ~/Views/Home.

### <a id="0.1__Toc274034225"></a>Nuovo supporto per la sintassi Razor di VBHTML

L'anteprima di ASP.NET MVC precedente includeva il supporto per le visualizzazioni C#che usano sintassi Razor basate su. Queste viste usano l'estensione di file. cshtml. Come parte del lavoro continuo per supportare Razor, ASP.NET MVC 3 beta introduce il supporto per la sintassi Razor in Visual Basic, che usa l'estensione di file vbhtml.

Per un'introduzione all'uso della sintassi Visual Basic nelle pagine VBHTML, vedere l'esercitazione all'URL seguente:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>Controllo più granulare su ValidateInputAttribute

ASP.NET MVC ha sempre incluso la classe ValidateInputAttribute, che richiama l'infrastruttura di convalida delle richieste ASP.NET Core per assicurarsi che la richiesta in ingresso non contenga input potenzialmente dannosi. Per impostazione predefinita, la convalida dell'input è abilitata. È possibile disabilitare la convalida delle richieste usando l'attributo ValidateInputAttribute, come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Molte applicazioni Web, tuttavia, dispongono di singoli campi modulo che devono consentire il codice HTML, mentre i campi rimanenti non lo sono. La classe ValidateInputAttribute consente ora di specificare un elenco di campi che non devono essere inclusi nella convalida della richiesta.

Se, ad esempio, si sviluppa un motore di Blog, è consigliabile consentire il markup nei campi corpo e riepilogo. Questi campi possono essere rappresentati da due elementi di input, ognuno con un attributo name corrispondente al nome della proprietà ("Body" e "Summary"). Per disabilitare solo la convalida delle richieste per questi campi, specificare i nomi (separati da virgola) nella proprietà Exclude della classe ValidateInput, come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>Gli helper convertono i caratteri di sottolineatura in trattini per i nomi di attributi HTML specificati usando oggetti anonimi

I metodi helper consentono di specificare le coppie nome/valore dell'attributo usando un oggetto anonimo, come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Questo approccio non consente di usare i trattini nel nome dell'attributo, perché non è possibile usare un trattino per un nome di proprietà in ASP.NET. Tuttavia, i trattini sono importanti per gli attributi HTML5 personalizzati; ad esempio, HTML5 usa il prefisso "data-".

Allo stesso tempo, non è possibile usare i caratteri di sottolineatura per i nomi di attributo in HTML, ma sono validi nei nomi di proprietà. Se pertanto si specificano gli attributi utilizzando un oggetto anonimo e se i nomi di attributo includono un carattere di sottolineatura, i metodi di supporto converteranno i caratteri di sottolineatura in trattini. Ad esempio, la sintassi Helper seguente usa un carattere di sottolineatura:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Nell'esempio precedente viene eseguito il rendering del markup seguente quando viene eseguito l'helper:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>Correzioni di bug

Il modello di oggetto predefinito per gli helper del modello EditorFor e DisplayFor ora supporta l'ordinamento specificato nella proprietà DisplayAttribute. Order. Nelle versioni precedenti l'impostazione ORDER non è stata utilizzata.

La convalida client supporta ora la convalida delle proprietà sottoposte a override con attributi di convalida applicati.

JsonValueProviderFactory è ora registrato per impostazione predefinita.

## <a id="0.1__Toc274034229"></a>Modifiche di rilievo

L'ordine di esecuzione dei filtri eccezioni è stato modificato per i filtri eccezioni con lo stesso valore di ordine. In ASP.NET MVC 2 e versioni precedenti, i filtri delle eccezioni sul controller con lo stesso ordine di quelli su un metodo di azione venivano eseguiti prima dei filtri delle eccezioni nel metodo di azione. Questa situazione si verifica in genere quando vengono applicati filtri eccezioni senza un valore di ordine specificato. In ASP.NET MVC 3 questo ordine è stato invertito in modo che il gestore di eccezioni più specifico venga eseguito per primo. Come nelle versioni precedenti, se la proprietà Order viene specificata in modo esplicito, i filtri vengono eseguiti nell'ordine specificato.

## <a id="0.1__Toc274034230"></a>Problemi noti

Durante l'installazione, la finestra di dialogo accettazione EULA Visualizza le condizioni di licenza in una finestra di dimensioni inferiori a quelle previste.

Le visualizzazioni Razor non dispongono del supporto IntelliSense né dell'evidenziazione della sintassi. Si prevede che il supporto per sintassi Razor in Visual Studio verrà incluso come parte di una versione successiva.

Quando si modifica una visualizzazione Razor (file cshtml), la <a id="0.1__Toc224729061"></a> <a id="0.1__Toc238051347"></a> voce di menu Vai al controller in Visual Studio non sarà disponibile e non ci sono frammenti di codice.

Quando si usa la sintassi @model per specificare una visualizzazione CSHTML fortemente tipizzata, i collegamenti specifici della lingua per i tipi non vengono riconosciuti. Ad esempio, @model int non funzionerà, ma @model Int32 funzionerà. La soluzione alternativa per questo bug consiste nell'usare il nome del tipo effettivo quando si specifica il tipo di modello.

Quando si usa la sintassi @model per specificare una visualizzazione CSHTML fortemente tipizzata (o @ModelType per specificare una visualizzazione VBHTML fortemente tipizzata), i tipi nullable e le dichiarazioni di matrice non sono supportati. Ad esempio, @model int? non è supportato. Usare invece `@model Nullable<Int32>`. Anche la sintassi @model String [] non è supportata. usare invece `@model IList<string>`.

Quando si aggiorna un progetto ASP.NET MVC 2 a ASP.NET MVC 3, assicurarsi di aggiungere quanto segue alla sezione appSettings del file Web. config:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Esiste un problema noto che fa in modo che l'autenticazione basata su form reindirizza sempre gli utenti non autenticati a ~/Account/Login, ignorando l'impostazione di autenticazione basata su form utilizzata in Web. config. La soluzione alternativa consiste nell'aggiungere l'impostazione dell'app seguente.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>Non responsabilità

© 2011 Microsoft Corporation. Tutti i diritti sono riservati. Questo documento viene fornito "così com'è". Le informazioni e le opinioni espresse nel presente documento, inclusi URL e altri riferimenti a siti Web Internet, possono cambiare senza preavviso. L'utente le utilizza a proprio rischio.

Questo documento non fornisce alcun diritto legale su qualsiasi proprietà intellettuale di un qualsiasi prodotto Microsoft. È possibile copiare e utilizzare questo documento per scopi personali o come riferimento.
