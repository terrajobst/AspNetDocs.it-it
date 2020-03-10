---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: "#2 iterazione: rendere l'aspetto dell'applicazioneC#gradevole () | Microsoft Docs"
author: microsoft
description: In questa iterazione, si migliora l'aspetto dell'applicazione modificando la pagina master e il foglio di stile CSS predefiniti della visualizzazione MVC ASP.NET.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: 246cb4b4668339cc4b7e4e03ea005102c6a2a5c3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602018"
---
# <a name="iteration-2--make-the-application-look-nice-c"></a>#2 iterazione: rendere l'aspetto dell'applicazioneC#gradevole ()

[Microsoft](https://github.com/microsoft)

[Scarica codice](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> In questa iterazione, si migliora l'aspetto dell'applicazione modificando la pagina master e il foglio di stile CSS predefiniti della visualizzazione MVC ASP.NET.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Compilazione di un'applicazione MVC di gestione deiC#contatti (ASP.NET)

In questa serie di esercitazioni viene creata un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di archiviare i nomi delle informazioni di contatto, i numeri di telefono e gli indirizzi di posta elettronica per un elenco di persone.

L'applicazione viene compilata su più iterazioni. Ogni iterazione migliora gradualmente l'applicazione. L'obiettivo di questo approccio di iterazione multipla è quello di consentire di comprendere il motivo di ogni modifica.

- #1 iterazione: creare l'applicazione. Nella prima iterazione, il gestore contatti viene creato nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base sul database: creazione, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2: rendere l'applicazione un aspetto gradevole. In questa iterazione, si migliora l'aspetto dell'applicazione modificando la pagina master e il foglio di stile CSS predefiniti della visualizzazione MVC ASP.NET.

- #3 iterazione: aggiungere la convalida del modulo. Nella terza iterazione viene aggiunta la convalida dei form di base. Si impedisce agli utenti di inviare un modulo senza completare i campi dei moduli richiesti. Vengono convalidati anche gli indirizzi di posta elettronica e i numeri di telefono.

- Iterazione #4: rendere l'applicazione a regime di controllo libero. In questa quarta iterazione sono disponibili diversi modelli di progettazione software che semplificano la gestione e la modifica dell'applicazione Contact Manager. Ad esempio, si effettua il refactoring dell'applicazione per usare il modello di repository e il modello di inserimento delle dipendenze.

- Iterazione #5-creare unit test. Nella quinta iterazione, l'applicazione viene semplificata per la manutenzione e la modifica mediante l'aggiunta di unit test. Si simulano le classi del modello di dati e si compilano unit test per i controller e la logica di convalida.

- #6 iterazione: usare lo sviluppo basato su test. In questa sesta iterazione vengono aggiunte nuove funzionalità all'applicazione scrivendo unit test prima e scrivendo il codice in base agli unit test. In questa iterazione vengono aggiunti gruppi di contatti.

- #7 iterazione: aggiungere la funzionalità AJAX. Nella settima iterazione, si migliora la velocità di risposta e le prestazioni dell'applicazione mediante l'aggiunta del supporto per AJAX.

## <a name="this-iteration"></a>Questa iterazione

L'obiettivo di questa iterazione è migliorare l'aspetto dell'applicazione Contact Manager. Attualmente, Contact Manager usa la pagina master della visualizzazione MVC ASP.NET predefinita e il foglio di stile CSS (vedere la figura 1). Questi non hanno un aspetto valido, ma non voglio che il responsabile del contatto sia simile a tutti gli altri siti Web ASP.NET MVC. Si desidera sostituire questi file con file personalizzati.

[![finestra di dialogo nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Figura 01**: l'aspetto predefinito di un'applicazione MVC ASP.NET ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-2-make-the-application-look-nice-cs/_static/image2.png))

In questa iterazione vengono illustrati due approcci per migliorare la progettazione visiva dell'applicazione. In primo luogo, viene illustrato come sfruttare i vantaggi di ASP.NET MVC Design Gallery per scaricare un modello di progettazione MVC ASP.NET gratuito. La raccolta di progetti MVC ASP.NET consente di creare un'applicazione Web professionale senza eseguire alcuna operazione.

Ho deciso di non usare un modello di ASP.NET MVC Design Gallery per l'applicazione Contact Manager. Al contrario, avevo un progetto personalizzato creato da una società di progettazione professionale. Nella seconda parte di questa esercitazione, spiegherò come ho collaborato con una società di progettazione professionale per creare la progettazione ASP.NET MVC finale.

## <a name="the-aspnet-mvc-design-gallery"></a>Raccolta di progettazione MVC ASP.NET

ASP.NET MVC Design Gallery è una risorsa gratuita fornita da Microsoft. La raccolta MVC ASP.NET è disponibile all'indirizzo seguente:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC Design Gallery ospita una raccolta di progetti di siti web gratuiti creati in modo specifico per l'uso in un progetto MVC ASP.NET. Le progettazioni vengono caricate dai membri della community. I visitatori della raccolta possono votare per i propri progetti preferiti (vedere la figura 2).

[![finestra di dialogo nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Figura 02**: ASP.NET MVC Design Gallery ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-2-make-the-application-look-nice-cs/_static/image4.png))

Durante la scrittura di questa esercitazione, la progettazione più diffusa nella raccolta è un progetto denominato ottobre di David Hauser. È possibile usare questa progettazione per un progetto MVC ASP.NET completando i passaggi seguenti:

1. Fare clic sul pulsante **download** per scaricare il file zip di ottobre nel computer.
2. Fare clic con il pulsante destro del mouse sul file zip di ottobre scaricato e scegliere il pulsante **Sblocca** (vedere la figura 3).
3. Decomprimere il file in una cartella denominata ottobre.
4. Selezionare tutti i file dalla cartella DesignTemplate contenuta nella cartella ottobre, fare clic con il pulsante destro del mouse sui file e selezionare l'opzione di menu **copia**.
5. Fare clic con il pulsante destro del mouse sul nodo del progetto ContactManager nella finestra di Esplora soluzioni di Visual Studio e selezionare l'opzione di menu **Incolla** (vedere la figura 4).
6. Selezionare l'opzione di menu Visual Studio **modifica, trova e Sostituisci, Sostituisci** e Sostituisci *[NomeProgetto]* con *ContactManager* (vedere la figura 5).

[![finestra di dialogo nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Figura 03**: sbloccare un file scaricato dal Web ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-2-make-the-application-look-nice-cs/_static/image6.png))

[![finestra di dialogo nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Figura 04**: sovrascrivere i file nella Esplora soluzioni ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-2-make-the-application-look-nice-cs/_static/image8.png))

[![finestra di dialogo nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Figura 05**: sostituzione di [NomeProgetto] con ContactManager ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-2-make-the-application-look-nice-cs/_static/image10.png))

Dopo aver completato questi passaggi, l'applicazione Web utilizzerà la nuova progettazione. La pagina della figura 6 illustra l'aspetto dell'applicazione Contact Manager con la progettazione di ottobre.

[![finestra di dialogo nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Figura 06**: ContactManager con il modello di ottobre ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-2-make-the-application-look-nice-cs/_static/image12.png))

## <a name="creating-a-custom-aspnet-mvc-design"></a>Creazione di una progettazione ASP.NET MVC personalizzata

ASP.NET MVC Design Gallery offre una selezione efficace di diversi stili di progettazione. La raccolta fornisce un modo indolore per personalizzare l'aspetto delle applicazioni MVC ASP.NET. Ovviamente, la raccolta ha il grande vantaggio di essere completamente gratuita.

Tuttavia, potrebbe essere necessario creare una progettazione completamente univoca per il sito Web. In tal caso, è opportuno lavorare con una società di progettazione di siti Web. Ho deciso di adottare questo approccio per la progettazione dell'applicazione Contact Manager.

Il gestore contatti è stato compresso dall'iterazione #1 e il progetto è stato inviato alla società di progettazione. Non sono proprietari di Visual Studio (vergogna), ma questo non ha riscontrato un problema. Sono stati in grado di scaricare gratuitamente Microsoft Visual Web Developer dal sito Web di [https://www.asp.net](https://www.asp.net) e di aprire l'applicazione Contact Manager in Visual Web Developer. In un paio di giorni, ha prodotto la progettazione nella figura 7.

[![finestra di dialogo nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Figura 07**: progettazione di Contact Manager di ASP.NET MVC ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-2-make-the-application-look-nice-cs/_static/image14.png))

Il nuovo progetto era costituito da due file principali: un nuovo file di foglio di stile CSS e un nuovo file di pagina master di visualizzazione. Una pagina master di visualizzazione contiene il layout e il contenuto condiviso per le visualizzazioni in un'applicazione MVC ASP.NET. Ad esempio, la pagina master di visualizzazione include l'intestazione, le schede di navigazione e il piè di pagina visualizzati nella figura 7. Ho sovrascritto la pagina master della visualizzazione site. master esistente nella cartella Views\Shared con il nuovo file site. master dell'azienda di progettazione,

La società di progettazione ha inoltre creato un nuovo foglio di stile CSS e un set di immagini. Ho inserito questi nuovi file nella cartella Content e ho sovrascritto il file site. CSS esistente. È necessario inserire tutto il contenuto statico nella cartella contenuto.

Si noti che la nuova progettazione per Contact Manager include immagini per la modifica e l'eliminazione di contatti. Accanto a ogni contatto della tabella dei contatti HTML verrà visualizzata un'immagine di modifica ed eliminazione.

Originariamente, i collegamenti di cui veniva eseguito il rendering con il codice HTML. Helper ActionLink () simile al seguente:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Il metodo HTML. ActionLink () non supporta le immagini (il metodo HTML codifica il testo del collegamento per motivi di sicurezza). Quindi, ho sostituito le chiamate a HTML. ActionLink () con le chiamate a URL. Action () come segue:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Il metodo HTML. ActionLink () esegue il rendering di un intero collegamento ipertestuale HTML. Il metodo URL. Action (), invece, esegue il rendering solo dell'URL senza il &lt;un tag di&gt;.

Si noti inoltre che nella nuova progettazione sono incluse sia le schede selezionate che quelle deselezionate. Nella figura 8, ad esempio, è selezionata la scheda **Crea nuovo contatto** e la scheda **contatti personali** non è selezionata.

[![finestra di dialogo nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Figura 08**: schede selezionate e deselezionate ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-2-make-the-application-look-nice-cs/_static/image16.png))

Per supportare il rendering delle schede selezionate e deselezionate, ho creato un helper HTML personalizzato denominato MenuItemHelper. Questo metodo helper esegue il rendering di un tag di &lt;li&gt; o di un &lt;li class = "Selected"&gt; tag a seconda che il controller e l'azione correnti corrispondano al nome del controller e dell'azione passato all'helper. Il codice per MenuItemHelper è contenuto nel listato 1.

**Listato 1-Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

Il MenuItemHelper usa internamente la classe TagBuilder per compilare il tag HTML &lt;li&gt;. La classe TagBuilder è una classe di utilità molto utile che è possibile usare ogni volta che è necessario creare un nuovo tag HTML. Include metodi per l'aggiunta di attributi, l'aggiunta di classi CSS, la generazione di ID e la modifica del codice HTML interno del tag.

## <a name="summary"></a>Riepilogo

In questa iterazione è stata migliorata la progettazione visiva dell'applicazione ASP.NET MVC. In primo luogo, è stata introdotta la raccolta di progetti MVC ASP.NET. Si è appreso come scaricare modelli di progettazione gratuiti dalla raccolta di progetti MVC ASP.NET che è possibile usare nelle applicazioni ASP.NET MVC.

Successivamente, è stato descritto come creare una progettazione personalizzata modificando il file del foglio di stile CSS e il file di pagina della visualizzazione master predefiniti. Per supportare la nuova progettazione, è necessario apportare alcune modifiche minime all'applicazione Contact Manager. Ad esempio, è stato aggiunto un nuovo helper HTML denominato MenuItemHelper che visualizza le schede selezionate e deselezionate.

Nell'iterazione successiva si affronterà l'argomento molto importante della convalida. Il codice di convalida viene aggiunto all'applicazione in modo che un utente non possa creare un nuovo contatto senza specificare i valori richiesti, ad esempio il nome e il cognome di una persona.

> [!div class="step-by-step"]
> [Precedente](iteration-1-create-the-application-cs.md)
> [Successivo](iteration-3-add-form-validation-cs.md)
