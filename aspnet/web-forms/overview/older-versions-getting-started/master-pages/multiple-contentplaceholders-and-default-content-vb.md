---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: Più ContentPlaceHolders e contenuto predefinito (VB) | Microsoft Docs
author: rick-anderson
description: Viene esaminato come aggiungere più titolari di posizione del contenuto a una pagina master e come specificare il contenuto predefinito nei titolari dei contenuti.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: b71cacb143094dcc5cf483c69c2fcc0f10def51c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74628683"
---
# <a name="multiple-contentplaceholders-and-default-content-vb"></a>Più elementi ContentPlaceHolder e contenuto predefinito (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Viene esaminato come aggiungere più titolari di posizione del contenuto a una pagina master e come specificare il contenuto predefinito nei titolari dei contenuti.

## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente è stato esaminato il modo in cui le pagine master consentono agli sviluppatori ASP.NET di creare un layout coerente a livello di sito. Le pagine master definiscono sia il markup comune a tutte le pagine di contenuto che le aree personalizzabili in base alla pagina. Nell'esercitazione precedente è stata creata una pagina master semplice (`Site.master`) e due pagine di contenuto (`Default.aspx` e `About.aspx`). La pagina master è costituita da due ContentPlaceHolders denominate `head` e `MainContent`, che si trovavano rispettivamente nell'elemento `<head>` e nel Web Form. Mentre le pagine di contenuto hanno ciascuno due controlli contenuto, viene specificato solo markup per quello corrispondente a `MainContent`.

Come evidenziato dai due controlli ContentPlaceHolder in `Site.master`, una pagina master può contenere più ContentPlaceHolders. È possibile che la pagina master specifichi il markup predefinito per i controlli ContentPlaceHolder. Una pagina di contenuto, quindi, può specificare facoltativamente il proprio markup o usare il markup predefinito. In questa esercitazione si osserverà l'uso di più controlli contenuto nella pagina master e si vedrà come definire il markup predefinito nei controlli ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Passaggio 1: aggiunta di controlli ContentPlaceHolder aggiuntivi alla pagina master

Molti progetti di siti Web contengono diverse aree sullo schermo che vengono personalizzate in base alle singole pagine. `Site.master`, la pagina master creata nell'esercitazione precedente, contiene un singolo ContentPlaceHolder all'interno del Web Form denominato `MainContent`. In particolare, questo ContentPlaceHolder si trova all'interno dell'elemento `mainContent` `<div>`.

La figura 1 Mostra `Default.aspx` quando viene visualizzato tramite un browser. L'area racchiusa in rosso è il markup specifico della pagina corrispondente a `MainContent`.

[![area rotonda Mostra l'area attualmente personalizzabile in base alla pagina](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Figura 01**: l'area di cerchio Mostra l'area attualmente personalizzabile in base alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))

Si supponga che, oltre all'area illustrata nella figura 1, sia necessario aggiungere elementi specifici della pagina alla colonna sinistra sotto le sezioni lezioni e notizie. A tale scopo, si aggiunge un altro controllo ContentPlaceHolder alla pagina master. Per seguire la procedura, aprire la pagina master di `Site.master` in Visual Web Developer, quindi trascinare un controllo ContentPlaceHolder dalla casella degli strumenti nella finestra di progettazione dopo la sezione notizie. Impostare il `ID` di ContentPlaceHolder su `LeftColumnContent`.

[![aggiungere un controllo ContentPlaceHolder alla colonna a sinistra della pagina master](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Figura 02**: aggiungere un controllo ContentPlaceHolder alla colonna sinistra della pagina master ([fare clic per visualizzare l'immagine con dimensioni complete](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))

Con l'aggiunta del `LeftColumnContent` ContentPlaceHolder alla pagina master, è possibile definire il contenuto per l'area in base alla pagina, includendo un controllo contenuto nella pagina la cui `ContentPlaceHolderID` è impostata su `LeftColumnContent`. Questo processo è esaminato nel passaggio 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Passaggio 2: definizione del contenuto per il nuovo ContentPlaceHolder nelle pagine di contenuto

Quando si aggiunge una nuova pagina contenuto al sito Web, tramite Visual Web Developer viene creato automaticamente un controllo contenuto nella pagina per ogni ContentPlaceHolder nella pagina master selezionata. Dopo aver aggiunto una `LeftColumnContent` ContentPlaceHolder alla pagina master nel passaggio 1, le nuove pagine di ASP.NET avranno ora tre controlli contenuto.

Per illustrare questo problema, aggiungere una nuova pagina contenuto alla directory radice denominata `MultipleContentPlaceHolders.aspx` associata alla pagina master `Site.master`. Visual Web Developer crea questa pagina con il markup dichiarativo seguente:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

Immettere contenuto nel controllo contenuto che fa riferimento al `MainContent` ContentPlaceHolders (`Content2`). Aggiungere quindi il markup seguente al controllo contenuto `Content3` (che fa riferimento al `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Dopo aver aggiunto questo markup, visitare la pagina tramite un browser. Come illustrato nella figura 3, il markup inserito nel controllo del contenuto `Content3` viene visualizzato nella colonna a sinistra sotto la sezione News (cerchio in rosso). Il markup inserito in `Content2` viene visualizzato nella parte destra della pagina (cerchio in blu).

[![colonna a sinistra include ora contenuto specifico della pagina sotto la sezione Notizie](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Figura 03**: la colonna a sinistra include ora contenuto specifico della pagina sotto la sezione News ([fare clic per visualizzare l'immagine con dimensioni complete](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))

### <a name="defining-content-in-existing-content-pages"></a>Definizione del contenuto nelle pagine di contenuto esistenti

La creazione di una nuova pagina contenuto incorpora automaticamente il controllo ContentPlaceHolder aggiunto nel passaggio 1. Tuttavia, le due pagine di contenuto esistenti, `About.aspx` e `Default.aspx`, non dispongono di un controllo del contenuto per il `LeftColumnContent` ContentPlaceHolder. Per specificare il contenuto per questo ContentPlaceHolder in queste due pagine esistenti, è necessario aggiungere un controllo contenuto.

Diversamente dalla maggior parte dei controlli Web ASP.NET, la casella degli strumenti di Visual Web Developer non include un elemento di controllo del contenuto. È possibile digitare manualmente il markup dichiarativo del controllo contenuto nella visualizzazione origine, ma un approccio più semplice e rapido consiste nell'usare la visualizzazione progettazione. Aprire la pagina `About.aspx` e passare alla visualizzazione progettazione. Come illustrato nella figura 4, la `LeftColumnContent` ContentPlaceHolder viene visualizzata nella visualizzazione progettazione; Se si passa il mouse su di essa, viene visualizzato il titolo "LeftColumnContent (Master)". L'inclusione di "Master" nel titolo indica che non è presente alcun controllo contenuto definito nella pagina per questo ContentPlaceHolder. Se è presente un controllo contenuto per ContentPlaceHolder, come nel caso di `MainContent`, il titolo leggerà: "*ContentPlaceHolderID* (Custom)".

Per aggiungere un controllo contenuto per il `LeftColumnContent` ContentPlaceHolder `About.aspx`, espandere lo smart tag di ContentPlaceHolder e fare clic sul collegamento Crea contenuto personalizzato.

[![visualizzazione progettazione per about. aspx Mostra LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Figura 04**: la visualizzazione progettazione per `About.aspx` mostra la `LeftColumnContent` ContentPlaceHolder ([fare clic per visualizzare l'immagine con dimensioni complete](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))

Facendo clic sul collegamento Crea contenuto personalizzato viene generato il controllo contenuto necessario nella pagina e la relativa proprietà `ContentPlaceHolderID` viene impostata sul `ID`di ContentPlaceHolder. Se ad esempio si fa clic sul collegamento Crea contenuto personalizzato per `LeftColumnContent` area in `About.aspx`, alla pagina viene aggiunto il markup dichiarativo seguente:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Omissione di controlli contenuto

Per ASP.NET non è necessario che tutte le pagine di contenuto includano i controlli contenuto per ogni ContentPlaceHolder definito nella pagina master. Se un controllo contenuto viene omesso, il motore ASP.NET utilizza il markup definito all'interno di ContentPlaceHolder nella pagina master. Questo markup viene definito *contenuto predefinito* di ContentPlaceHolder ed è utile negli scenari in cui il contenuto di una determinata area è comune tra la maggior parte delle pagine, ma deve essere personalizzato per un numero ridotto di pagine. Il passaggio 3 esamina la specifica del contenuto predefinito nella pagina master.

Attualmente, `Default.aspx` contiene due controlli contenuto per la `head` e `MainContent` ContentPlaceHolders; non dispone di un controllo contenuto per `LeftColumnContent`. Di conseguenza, quando viene eseguito il rendering di `Default.aspx` viene utilizzato il contenuto predefinito di `LeftColumnContent` ContentPlaceHolder. Poiché è ancora necessario definire un contenuto predefinito per questo ContentPlaceHolder, il risultato finale è che non viene emesso alcun markup per questa area. Per verificare questo comportamento, visitare `Default.aspx` tramite un browser. Come illustrato nella figura 5, nessun markup viene emesso nella colonna sinistra sotto la sezione News.

[![non viene eseguito il rendering del contenuto per il ContentPlaceHolder LeftColumnContent](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Figura 05**: non viene eseguito il rendering del contenuto per il `LeftColumnContent` ContentPlaceHolder ([fare clic per visualizzare l'immagine con dimensioni complete](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))

## <a name="step-3-specifying-default-content-in-the-master-page"></a>Passaggio 3: specifica del contenuto predefinito nella pagina master

Alcune progettazioni di siti Web includono un'area il cui contenuto è lo stesso per tutte le pagine del sito eccetto una o due eccezioni. Si prenda in considerazione un sito Web che supporta gli account utente. Un sito di questo tipo richiede una pagina di accesso in cui i visitatori possono immettere le proprie credenziali per accedere al sito. Per accelerare il processo di accesso, i progettisti di siti Web possono includere caselle di testo nome utente e password nell'angolo superiore sinistro di ogni pagina per consentire agli utenti di accedere senza dover visitare esplicitamente la pagina di accesso. Sebbene queste caselle di testo per nome utente e password siano utili nella maggior parte delle pagine, sono ridondanti nella pagina di accesso, che contiene già le caselle di testo per le credenziali dell'utente.

Per implementare questa progettazione, è possibile creare un controllo ContentPlaceHolder nell'angolo superiore sinistro della pagina master. Ogni pagina che doveva visualizzare le caselle di testo nome utente e password nell'angolo superiore sinistro creerebbe un controllo contenuto per questo ContentPlaceHolder e aggiungerà l'interfaccia necessaria. La pagina di accesso, d'altra parte, ometterebbe l'aggiunta di un controllo contenuto per questo ContentPlaceHolder o creerebbe un controllo contenuto senza markup definito. Lo svantaggio di questo approccio è che è necessario ricordare di aggiungere le caselle di testo nome utente e password a ogni pagina aggiunta al sito (ad eccezione della pagina di accesso). Questa operazione richiede problemi. È probabile che si dimentichi di aggiungere queste caselle di testo a una pagina o a due o, peggio, che l'interfaccia non venga implementata correttamente (forse aggiungendo solo una casella di testo anziché due).

Una soluzione migliore consiste nel definire le caselle di testo nome utente e password come contenuto predefinito di ContentPlaceHolder. In questo modo, è necessario eseguire l'override di questo contenuto predefinito solo nelle poche pagine in cui non vengono visualizzate le caselle di testo nome utente e password (ad esempio, la pagina di accesso). Per illustrare come specificare il contenuto predefinito per un controllo ContentPlaceHolder, è necessario implementare lo scenario appena illustrato.

> [!NOTE]
> Il resto di questa esercitazione aggiorna il sito Web in modo da includere un'interfaccia di accesso nella colonna a sinistra per tutte le pagine, ma la pagina di accesso. Tuttavia, in questa esercitazione non viene esaminato come configurare il sito Web per supportare gli account utente. Per ulteriori informazioni su questo argomento, fare riferimento alle esercitazioni su [autenticazione basata su form, autorizzazione, account utente e ruoli](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) .

### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Aggiunta di un ContentPlaceHolder e specifica del contenuto predefinito

Aprire la pagina master di `Site.master` e aggiungere il markup seguente alla colonna sinistra tra l'etichetta `DateDisplay` e la sezione Lessons:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Una volta aggiunto questo markup, la visualizzazione della struttura della pagina master avrà un aspetto simile a quello della figura 6.

[![la pagina master include un controllo di accesso](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Figura 06**: la pagina master include un controllo di accesso ([fare clic per visualizzare l'immagine con dimensioni complete](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))

Questo ContentPlaceHolder, `QuickLoginUI`, dispone di un controllo Web di accesso come contenuto predefinito. Il controllo Login Visualizza un'interfaccia utente che richiede all'utente il nome utente e la password insieme a un pulsante Accedi. Quando si fa clic sul pulsante Accedi, il controllo di accesso convalida internamente le credenziali dell'utente rispetto all'API di appartenenza. Per utilizzare questo controllo di accesso in pratica, è necessario configurare il sito per l'utilizzo dell'appartenenza. Questo argomento esula dall'ambito di questa esercitazione. Per ulteriori informazioni sulla compilazione di un'applicazione Web che supporta gli account utente, vedere l'articolo relativo all' [autenticazione basata su form, all'autorizzazione, agli account utente e ai ruoli](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) .

È possibile personalizzare il comportamento o l'aspetto del controllo di accesso. Sono state impostate due proprietà: `TitleText` e `FailureAction`. Il valore della proprietà `TitleText`, il cui valore predefinito è "log in", viene visualizzato nella parte superiore dell'interfaccia utente del controllo. Questa proprietà è stata impostata in modo da visualizzare il testo "Sign in" come elemento `<h3>`. La proprietà `FailureAction` indica cosa fare se le credenziali dell'utente non sono valide. Il valore predefinito è `Refresh`, che lascia l'utente nella stessa pagina e visualizza un messaggio di errore all'interno del controllo di accesso. Lo ho modificato in `RedirectToLoginPage`, che invia l'utente alla pagina di accesso in caso di credenziali non valide. Preferisco inviare l'utente alla pagina di accesso quando un utente tenta di eseguire l'accesso da un'altra pagina, ma ha esito negativo perché la pagina di accesso può contenere altre istruzioni e opzioni che non rientrano facilmente nella colonna a sinistra. La pagina di accesso, ad esempio, può includere opzioni per recuperare una password dimenticata o per creare un nuovo account.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Creazione della pagina di accesso ed override del contenuto predefinito

Con la pagina master completata, il passaggio successivo consiste nel creare la pagina di accesso. Aggiungere una pagina ASP.NET alla directory radice del sito denominata `Login.aspx`, associarla alla pagina master del `Site.master`. In questo modo viene creata una pagina con quattro controlli contenuto, uno per ogni ContentPlaceHolders definito in `Site.master`.

Aggiungere un controllo Login al controllo contenuto `MainContent`. Analogamente, è possibile aggiungere qualsiasi contenuto all'area `LeftColumnContent`. Tuttavia, assicurarsi di lasciare vuoto il controllo contenuto per la `QuickLoginUI` ContentPlaceHolder. In questo modo, il controllo di accesso non verrà visualizzato nella colonna sinistra della pagina di accesso.

Dopo aver definito il contenuto per le aree `MainContent` e `LeftColumnContent`, il markup dichiarativo della pagina di accesso dovrebbe essere simile al seguente:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

Nella figura 7 Questa pagina viene visualizzata in un browser. Poiché in questa pagina viene specificato un controllo contenuto per il `QuickLoginUI` ContentPlaceHolder, viene eseguito l'override del contenuto predefinito specificato nella pagina master. Il risultato finale è che il controllo di accesso visualizzato nella visualizzazione di progettazione della pagina master (vedere la figura 6) non viene sottoposto a rendering in questa pagina.

[![la pagina di accesso comprime il contenuto predefinito di QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Figura 07**: la pagina di accesso comprime il contenuto predefinito di `QuickLoginUI` ContentPlaceHolder ([fare clic per visualizzare l'immagine con dimensioni complete](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))

### <a name="using-the-default-content-in-new-pages"></a>Uso del contenuto predefinito in nuove pagine

Si desidera visualizzare il controllo account di accesso nella colonna sinistra per tutte le pagine ad eccezione della pagina di accesso. A tale scopo, tutte le pagine di contenuto ad eccezione della pagina di accesso devono omettere un controllo contenuto per il `QuickLoginUI` ContentPlaceHolder. Omettendo un controllo contenuto, verrà utilizzato il contenuto predefinito di ContentPlaceHolder.

Le pagine di contenuto esistenti, `Default.aspx`, `About.aspx`e `MultipleContentPlaceHolders.aspx`, non includono un controllo contenuto per `QuickLoginUI` perché sono state create prima di aggiungere il controllo ContentPlaceHolder alla pagina master. Non è pertanto necessario aggiornare le pagine esistenti. Tuttavia, per impostazione predefinita, le nuove pagine aggiunte al sito Web includono un controllo contenuto per il `QuickLoginUI` ContentPlaceHolder. Quindi, dobbiamo ricordare di rimuovere questi controlli contenuto ogni volta che viene aggiunta una nuova pagina di contenuto (a meno che non si desideri eseguire l'override del contenuto predefinito di ContentPlaceHolder, come nel caso della pagina di accesso).

Per rimuovere il controllo contenuto, è possibile eliminare manualmente il markup dichiarativo dalla visualizzazione origine o, dalla visualizzazione progettazione, scegliere il collegamento predefinito al contenuto del master dallo smart tag. Uno degli approcci rimuove il controllo contenuto dalla pagina e produce lo stesso effetto netto.

Nella figura 8 viene illustrato `Default.aspx` quando viene visualizzato tramite un browser. Tenere presente che `Default.aspx` dispone solo di due controlli contenuto specificati nel markup dichiarativo, uno per `head` e uno per `MainContent`. Viene quindi visualizzato il contenuto predefinito per il `LeftColumnContent` e `QuickLoginUI` ContentPlaceHolders.

[![viene visualizzato il contenuto predefinito per LeftColumnContent e QuickLoginUI ContentPlaceHolders](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Figura 08**: viene visualizzato il contenuto predefinito per il `LeftColumnContent` e `QuickLoginUI` ContentPlaceHolders ([fare clic per visualizzare l'immagine con dimensioni complete](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))

## <a name="summary"></a>Riepilogo

Il modello di pagina master ASP.NET consente un numero arbitrario di ContentPlaceHolders nella pagina master. ContentPlaceHolders include il contenuto predefinito, che viene generato nel caso in cui non sia presente alcun controllo contenuto corrispondente nella pagina contenuto. In questa esercitazione è stato illustrato come includere altri controlli ContentPlaceHolder nella pagina master e come definire i controlli contenuto per questi nuovi ContentPlaceHolders nelle pagine ASP.NET nuove ed esistenti. È stato inoltre esaminato come specificare il contenuto predefinito in un ContentPlaceHolder, che risulta utile negli scenari in cui solo una minoranza di pagine deve personalizzare il contenuto standardizzato in modo diverso all'interno di una determinata area.

Nell'esercitazione successiva si esaminerà il `head` ContentPlaceHolder in modo più dettagliato, in cui viene illustrato come definire in modo dichiarativo e a livello di codice il titolo, i tag meta e altre intestazioni HTML in base alla pagina.

Buona programmazione!

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 3,5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott può essere raggiunto in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore del lead per questa esercitazione è stato. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](creating-a-site-wide-layout-using-master-pages-vb.md)
> [Successivo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
