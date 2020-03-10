---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Riutilizzare l'interfaccia utente tramite pagine master e parziali | Microsoft Docs
author: microsoft
description: Il passaggio 7 analizza i modi in cui è possibile applicare il principio "DRY" all'interno dei modelli di vista per eliminare la duplicazione del codice, usando modelli di visualizzazione parziali e pagine master.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0b17cb6ac14b7f187bf1f175097a37907689d46e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580332"
---
# <a name="re-use-ui-using-master-pages-and-partials"></a>Riutilizzare l'interfaccia utente tramite pagine master e visualizzazioni parziali

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 7 di un' [applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuita che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.
> 
> Il passaggio 7 analizza i modi in cui è possibile applicare il "principio secco" all'interno dei modelli di vista per eliminare la duplicazione del codice, usando modelli di visualizzazione parziali e pagine master.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner passaggio 7: parti parziali e pagine master

Una delle filosofie di progettazione ASP.NET MVC abbraccia è il principio "non ripetere se stessi", comunemente definito "asciutto". Una progettazione ASCIUTta consente di eliminare la duplicazione del codice e della logica, che in ultima analisi rende più veloce la compilazione e la gestione delle applicazioni.

Abbiamo già visto il principio secco applicato in diversi scenari NerdDinner. Alcuni esempi: la logica di convalida è implementata all'interno del livello del modello, che ne consente l'applicazione in scenari di modifica e creazione nel controller. viene riutilizzato il modello di visualizzazione "NotFound" nei metodi di azione modifica, dettagli ed Elimina; viene usato un modello di denominazione delle convenzioni con i modelli di visualizzazione, che elimina la necessità di specificare in modo esplicito il nome quando si chiama il metodo helper View (); si sta riutilizzando la classe DinnerFormViewModel per gli scenari di azione di modifica e creazione.

Verranno ora esaminati i modi in cui è possibile applicare il "principio secco" all'interno dei modelli di vista per eliminare anche la duplicazione del codice.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Nuova visita ai modelli di visualizzazione di modifica e creazione

Attualmente si usano due modelli di vista diversi, ovvero "Edit. aspx" e "create. aspx", per visualizzare l'interfaccia utente del modulo dinner. Un rapido confronto visivo di questi elementi evidenzia quanto è simile. Di seguito è riportato il modulo di creazione:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Ecco come appare il modulo "Edit":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Non c'è molta differenza? Ad eccezione del titolo e del testo dell'intestazione, il layout del form e i controlli di input sono identici.

Se si aprono i modelli di visualizzazione "Edit. aspx" e "create. aspx", si noterà che contengono il layout del modulo e il codice di controllo di input identici. Questa duplicazione comporta la necessità di apportare modifiche due volte in qualsiasi momento, introducendo o modificando una nuova proprietà Dinner, che non è corretta.

### <a name="using-partial-view-templates"></a>Utilizzo di modelli di visualizzazione parziale

ASP.NET MVC supporta la possibilità di definire modelli "visualizzazione parziale" che possono essere usati per incapsulare la logica di rendering della visualizzazione per una parte secondaria di una pagina. I "parziali" costituiscono un modo utile per definire la logica di rendering della visualizzazione una volta e quindi riutilizzarla in più punti all'interno di un'applicazione.

Per semplificare la creazione di una duplicazione del modello di visualizzazione Edit. aspx e create. aspx, è possibile creare un modello di visualizzazione parziale denominato "DinnerForm. ascx" che incapsula il layout del form e gli elementi di input comuni a entrambi. Questa operazione verrà eseguita facendo clic con il pulsante destro del mouse sulla directory/Views/Dinners e scegliendo il comando di menu "Add-&gt;View":

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione". Chiameremo la nuova visualizzazione che vogliamo creare "DinnerForm", selezionare la casella di controllo "crea una visualizzazione parziale" all'interno della finestra di dialogo e indicare che verrà passata una classe DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Quando si fa clic sul pulsante "Aggiungi", in Visual Studio viene creato un nuovo modello di visualizzazione "DinnerForm. ascx" nella directory "\Views\Dinners".

È quindi possibile copiare e incollare il codice del formato duplicato o il codice di controllo dell'input dai modelli di visualizzazione Edit. aspx/create. aspx nel nuovo modello di visualizzazione parziale "DinnerForm. ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

È quindi possibile aggiornare i modelli di modifica e creazione della vista per chiamare il modello parziale DinnerForm ed eliminare la duplicazione del form. È possibile eseguire questa operazione chiamando HTML. RenderPartial ("DinnerForm") nei modelli di visualizzazione:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

È possibile qualificare in modo esplicito il percorso del modello parziale desiderato quando si chiama HTML. RenderPartial (ad esempio: ~ views/dinners/DinnerForm. ascx "). Nel codice precedente, tuttavia, viene sfruttato il modello di denominazione basato sulle convenzioni all'interno di ASP.NET MVC e viene semplicemente specificato "DinnerForm" come nome dell'oggetto parziale di cui eseguire il rendering. Quando si esegue questa operazione, ASP.NET MVC apparirà per primo nella directory delle visualizzazioni basate sulle convenzioni (per DinnersController sarà/Views/Dinners). Se non trova il modello parziale, lo cercherà nella directory/Views/Shared.

Quando HTML. RenderPartial () viene chiamato solo con il nome della visualizzazione parziale, ASP.NET MVC passa alla visualizzazione parziale lo stesso modello e gli oggetti del dizionario ViewData utilizzati dal modello di visualizzazione chiamante. In alternativa, sono disponibili versioni di overload di HTML. RenderPartial () che consentono di passare un oggetto modello alternativo e/o un dizionario ViewData per la visualizzazione parziale da usare. Questa operazione è utile per gli scenari in cui si vuole solo passare un subset del modello completo/ViewModel.

| **Argomento laterale: perché &lt;%%&gt; invece di &lt;% =%&gt;?** |
| --- |
| Una delle operazioni più complesse che è possibile notare con il codice precedente è che si sta utilizzando un &lt;%%&gt; blocco anziché un blocco &lt;% =%&gt; quando si chiama HTML. RenderPartial (). &lt;% =%&gt; blocchi in ASP.NET indicano che uno sviluppatore desidera eseguire il rendering di un valore specificato (ad esempio, &lt;% = "Hello"%&gt; eseguire il rendering di "Hello"). &lt;%%&gt; blocchi indicano invece che lo sviluppatore vuole eseguire il codice e che qualsiasi output sottoposto a rendering deve essere eseguito in modo esplicito (ad esempio, &lt;% Response. Write ("Hello")%&gt;. Il motivo per cui viene usato un blocco &lt;%%&gt; con il codice HTML. RenderPartial precedente è perché il metodo HTML. RenderPartial () non restituisce una stringa e restituisce invece il contenuto direttamente al flusso di output del modello di visualizzazione chiamante. Questa operazione viene eseguita per motivi di efficienza delle prestazioni e, in questo modo, evita la necessità di creare un oggetto stringa temporaneo (potenzialmente molto grande). Questo riduce l'utilizzo della memoria e migliora la velocità effettiva complessiva dell'applicazione. Un errore comune quando si usa HTML. RenderPartial () consiste nel dimenticare di aggiungere un punto e virgola alla fine della chiamata quando si trova all'interno di un &lt;%%&gt; blocco. Questo codice, ad esempio, genera un errore del compilatore: &lt;% HTML. RenderPartial ("DinnerForm")%&gt; è invece necessario scrivere: &lt;% HTML. RenderPartial ("DinnerForm"); %&gt; ciò è dovuto al fatto che &lt;%%&gt; blocchi sono istruzioni di codice indipendenti e quando C# si utilizzano istruzioni di codice deve terminare con un punto e virgola. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Utilizzo di modelli di visualizzazione parziale per chiarire il codice

È stato creato il modello di visualizzazione parziale "DinnerForm" per evitare di duplicare la logica di rendering della visualizzazione in più punti. Questo è il motivo più comune per creare modelli di visualizzazione parziale.

In alcuni casi è comunque opportuno creare visualizzazioni parziali anche quando vengono chiamate solo in un'unica posizione. I modelli di visualizzazione molto complessi possono spesso diventare molto più facili da leggere quando la logica di rendering della visualizzazione viene estratta e partizionata in uno o più modelli parziali denominati.

Si consideri, ad esempio, il frammento di codice seguente dal file site. master del progetto (che verrà esaminato a breve). Il codice è relativamente semplice da leggere, in parte perché la logica per visualizzare un collegamento di accesso/disconnessione nella parte superiore destra della schermata è incapsulata all'interno della parte "LogOnUserControl" parziale:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Ogni volta che ci si trova a trovare un confuso tentativo di comprendere il markup HTML/codice all'interno di un modello di visualizzazione, valutare se non sarebbe più chiaro se ne venisse estratto e sottoposto a refactoring in visualizzazioni parziali ben denominate.

### <a name="master-pages"></a>Pagine master

Oltre a supportare le visualizzazioni parziali, ASP.NET MVC supporta anche la possibilità di creare modelli "pagina master" che possono essere usati per definire il layout comune e il codice HTML di primo livello di un sito. I controlli segnaposto del contenuto possono quindi essere aggiunti alla pagina master per identificare le aree sostituibili che possono essere sottoposte a override o "compilate" dalle viste. Questo fornisce un metodo molto efficace (e asciutto) per applicare un layout comune in un'applicazione.

Per impostazione predefinita, al nuovo progetto MVC ASP.NET è stato aggiunto automaticamente un modello di pagina master. Questa pagina master è denominata "site. master" e si trova all'interno della cartella \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Il file site. master predefinito è simile al seguente. Definisce il codice HTML esterno del sito, insieme a un menu per la navigazione nella parte superiore. Contiene due controlli segnaposto del contenuto sostituibili, uno per il titolo e l'altro per il quale deve essere sostituito il contenuto primario di una pagina:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Tutti i modelli di visualizzazione creati per l'applicazione NerdDinner ("list", "Details", "Edit", "create", "NotFound" e così via) sono basati sul modello site. master. Questo è indicato tramite l'attributo "MasterPageFile" che è stato aggiunto per impostazione predefinita alla prima direttiva &lt;% @ page%&gt; quando sono state create le visualizzazioni utilizzando la finestra di dialogo "Aggiungi visualizzazione":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Ciò significa che è possibile modificare il contenuto del sito. master e fare in modo che le modifiche vengano applicate e usate automaticamente quando si esegue il rendering dei modelli di visualizzazione.

Verrà ora aggiornata la sezione dell'intestazione site. Master in modo che l'intestazione dell'applicazione sia "NerdDinner" invece di "applicazione MVC". Aggiorniamo anche il menu di navigazione in modo che la prima scheda sia "Find a dinner" (gestita dal metodo di azione index () di HomeController) e aggiungiamo una nuova scheda denominata "host a dinner" (gestita dal metodo di azione Create () di DinnersController):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Quando si salva il file site. master e si aggiorna il browser, le modifiche dell'intestazione verranno visualizzate in tutte le visualizzazioni all'interno dell'applicazione. Esempio:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

E con l'URL */dinners/Edit/[ID]* :

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Passo successivo

I parziali e le pagine master offrono opzioni molto flessibili che consentono di organizzare in modo semplice le visualizzazioni. Si noterà che consentono di evitare la duplicazione di contenuto/codice di visualizzazione e di semplificare la lettura e la gestione dei modelli di visualizzazione.

A questo punto è possibile rivedere lo scenario di presentazione creato in precedenza e abilitare il supporto per il paging scalabile.

> [!div class="step-by-step"]
> [Precedente](use-viewdata-and-implement-viewmodel-classes.md)
> [Successivo](implement-efficient-data-paging.md)
