---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Pagine master | Microsoft Docs
author: microsoft
description: Uno dei componenti principali di un sito Web con successo è un aspetto coerente. In ASP.NET 1. x, gli sviluppatori usavano i controlli utente per replicare la pagina comune elem...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: 36f2caf7c2c9bcafd22c8f6681c1d6b19fe5078a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78567333"
---
# <a name="master-pages"></a>Pagine master

[Microsoft](https://github.com/microsoft)

> Uno dei componenti principali di un sito Web con successo è un aspetto coerente. In ASP.NET 1. x, gli sviluppatori usavano i controlli utente per replicare gli elementi della pagina comuni in un'applicazione Web. Sebbene questa sia sicuramente una soluzione praticabile, l'uso dei controlli utente presenta alcuni svantaggi. Ad esempio, una modifica alla posizione di un controllo utente richiede una modifica a più pagine in un sito. Anche i controlli utente non vengono sottoposti a rendering nella visualizzazione progettazione dopo essere stati inseriti in una pagina.

Uno dei componenti principali di un sito Web con successo è un aspetto coerente. In ASP.NET 1. x, gli sviluppatori usavano i controlli utente per replicare gli elementi della pagina comuni in un'applicazione Web. Sebbene questa sia sicuramente una soluzione praticabile, l'uso dei controlli utente presenta alcuni svantaggi. Ad esempio, una modifica alla posizione di un controllo utente richiede una modifica a più pagine in un sito. Anche i controlli utente non vengono sottoposti a rendering nella visualizzazione progettazione dopo essere stati inseriti in una pagina.

ASP.NET 2,0 introduce le pagine master come un modo per mantenere un aspetto coerente e, come si vedrà presto, le pagine master rappresentano un miglioramento significativo rispetto al metodo di controllo utente.

## <a name="why-master-pages"></a>Perché le pagine master?

È possibile chiedersi perché le pagine master sono necessarie in ASP.NET 2,0. Dopo tutto, gli sviluppatori di siti web usano già i controlli utente in ASP.NET 1. x per condividere le aree di contenuto tra le pagine. Esistono infatti diversi motivi per cui i controlli utente sono una soluzione meno ottimale per la creazione di un layout comune.

I controlli utente non definiscono effettivamente il layout di pagina. Definiscono invece il layout e la funzionalità per una parte di una pagina. La distinzione tra questi due è importante perché rende molto più difficile la gestibilità di una soluzione di controllo utente. Ad esempio, quando si desidera modificare la posizione di un controllo utente nella pagina, è necessario modificare la pagina effettiva in cui viene visualizzato il controllo utente. Si tratta di un problema se sono presenti solo poche pagine, ma in siti di grandi dimensioni, diventa rapidamente un incubo di gestione del sito.

Un altro svantaggio dell'uso dei controlli utente per la definizione di un layout comune è radicato nell'architettura di ASP.NET. Se viene modificato un membro pubblico di un controllo utente, è necessario ricompilare tutte le pagine che usano il controllo utente. A sua volta, ASP.NET utilizzerà nuovamente JIT le pagine quando si accede per la prima volta. Questo, ancora una volta, produce un'architettura non scalabile e un problema di gestione del sito per siti più grandi.

Entrambi questi problemi (e molti altri) sono ben trattati dalle pagine master in ASP.NET 2,0.

## <a name="how-master-pages-work"></a>Come funzionano le pagine master

Una pagina master è analoga a un modello per altre pagine. Gli elementi della pagina che devono essere condivisi tra le altre pagine, ad esempio i menu, i bordi e così via, vengono aggiunti alla pagina master. Quando vengono aggiunte nuove pagine al sito, è possibile associarle a una pagina master. Una pagina associata a una pagina master viene chiamata **pagina contenuto**. Per impostazione predefinita, una pagina di contenuto assume l'aspetto della pagina master. Tuttavia, quando si crea una pagina master, è possibile definire parti della pagina che possono essere sostituite dalla pagina contenuto con il relativo contenuto. Queste parti sono definite usando un nuovo controllo introdotto in ASP.NET 2,0; controllo **ContentPlaceHolder** .

Una pagina master può contenere un numero qualsiasi di controlli ContentPlaceHolder (o nessuno). Nella pagina contenuto, il contenuto dei controlli ContentPlaceHolder viene visualizzato all'interno di controlli **contenuto** , un altro nuovo controllo in ASP.NET 2,0. Per impostazione predefinita, i controlli contenuto delle pagine di contenuto sono vuoti, in modo che sia possibile fornire contenuto personalizzato. Se si vuole usare il contenuto della pagina master all'interno dei controlli contenuto, è possibile farlo come si vedrà più avanti in questo modulo. Il controllo contenuto viene mappato al controllo ContentPlaceHolder tramite l'attributo ContentPlaceHolderID del controllo contenuto. Il codice seguente esegue il mapping di un controllo contenuto a un controllo ContentPlaceHolder denominato mainBody in una pagina master.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Spesso le persone descrivono le pagine master come classe di base per altre pagine. In realtà non è vero. La relazione tra pagine master e pagine di contenuto non è un'ereditarietà.

La **Figura 1** Mostra una pagina master e una pagina di contenuto associata come appaiono in Visual Studio 2005. È possibile visualizzare il controllo ContentPlaceHolder nella pagina master e il controllo contenuto corrispondente nella pagina contenuto. Si noti che il contenuto delle pagine master esterno a ContentPlaceHolder è visibile ma non è disponibile nella pagina contenuto. Solo il contenuto all'interno del ContentPlaceHolder può essere sostituito dalla pagina contenuto. Tutti gli altri contenuti che provengono dalla pagina master non sono modificabili.

![Pagina master e pagina contenuto associata](master-pages/_static/image1.jpg)

**Figura 1**: pagina master e pagina contenuto associata

## <a name="creating-a-master-page"></a>Creazione di una pagina master

Per creare una nuova pagina master:

1. Aprire Visual Studio 2005 e creare un nuovo sito Web.
2. Fare clic su file, nuovo, file.
3. Scegliere file master dalla finestra di dialogo Aggiungi nuovo elemento, come illustrato nella **Figura 2**.
4. Fare clic su Aggiungi.

![Creazione di una nuova pagina master](master-pages/_static/image2.jpg)

**Figura 2**: creazione di una nuova pagina master

Si noti che l'estensione di file per una pagina master è *. master*. Questo è uno dei modi in cui una pagina master differisce da una pagina ordinaria. L'altra differenza principale consiste nel fatto che, al posto di una direttiva di @Page, la pagina master contiene una direttiva di @Master. Passare alla visualizzazione origine per la pagina master appena creata ed esaminare il codice.

Per impostazione predefinita, una nuova pagina master disporrà di un controllo ContentPlaceHolder. Nella maggior parte dei casi, è consigliabile creare prima gli elementi di pagina comuni e quindi inserire i controlli ContentPlaceHolder in cui si desidera il contenuto personalizzato. In questi casi, gli sviluppatori vorranno eliminare il controllo ContentPlaceHolder predefinito e inserirne di nuovi durante lo sviluppo della pagina. I controlli ContentPlaceHolder non sono ridimensionabili nonostante il fatto che visualizzino gli handle di ridimensionamento. Il controllo ContentPlaceHolder si ridimensiona automaticamente in base al contenuto che contiene con un'eccezione; Se si inserisce un controllo ContentPlaceHolder all'interno di un elemento Block, ad esempio una cella della tabella, tale controllo verrà ridimensionato in base alle dimensioni dell'elemento.

## <a name="lab-1-working-with-master-pages"></a>Lab 1 utilizzo di pagine master

In questo Lab verrà creata una nuova pagina master che definirà tre controlli ContentPlaceHolder. Si creerà quindi una nuova pagina di contenuto e si sostituirà il contenuto da almeno uno dei controlli ContentPlaceHolder.

1. Creare una pagina master e inserire i controlli ContentPlaceHolder. 

    1. Creare una nuova pagina master come descritto in precedenza.
    2. Eliminare il controllo ContentPlaceHolder predefinito.
    3. Selezionare il controllo ContentPlaceHolder facendo clic sul bordo superiore ombreggiato del controllo e quindi eliminarlo colpendo il tasto CANC sulla tastiera.
    4. Inserire una nuova tabella usando l' *intestazione e* il modello laterale, come illustrato nella figura 3. Modificare la larghezza e l'altezza fino al 90%, in modo che l'intera tabella sia visibile nella finestra di progettazione.

![](master-pages/_static/image3.jpg)

**Figura 3**

1. Posizionare il cursore in ogni cella della tabella e impostare la proprietà *valign* su *Top*.
2. Dalla casella degli strumenti inserire un controllo ContentPlaceHolder nella cella superiore della tabella, ovvero la cella dell'intestazione.
3. Quando si inserisce questo controllo ContentPlaceHolder, si noterà che l'altezza della riga prenderà quasi tutta la pagina, come illustrato nella figura 4. A questo punto non c'è da preoccuparsi.

![Lo spazio vuoto si trova nella stessa cella del ContentPlaceHolder](master-pages/_static/image1.gif)

**Figura 4**: lo spazio vuoto si trova nella stessa cella del ContentPlaceHolder

1. Inserire un controllo ContentPlaceHolder nelle altre due celle. Una volta inseriti gli altri controlli ContentPlaceHolder, le dimensioni delle celle della tabella devono essere le stesse come previsto. La pagina dovrebbe ora apparire come la pagina illustrata nella **Figura 5**.

![Master con tutti i controlli ContentPlaceHolder. Si noti che l'altezza della cella per la cella dell'intestazione è ora quella che dovrebbe essere](master-pages/_static/image2.gif)

**Figura 5**: Master con tutti i controlli ContentPlaceHolder. Si noti che l'altezza della cella per la cella dell'intestazione è ora quella che dovrebbe essere

1. Immettere il testo desiderato in ognuno dei tre controlli ContentPlaceHolder.
2. Salvare la pagina master come exercise1. master.
3. Creare un nuovo Web Form e associarlo alla pagina master exercise1. master.
4. Selezionare file, nuovo, file in Visual Studio 2005.
5. Selezionare **Web Form** nella finestra di dialogo Aggiungi nuovo elemento.
6. Assicurarsi che la casella di controllo Seleziona pagina master sia selezionata, come illustrato nella figura 6.

![Aggiunta di una nuova pagina contenuto](master-pages/_static/image3.gif)

**Figura 6**: aggiunta di una nuova pagina di contenuto

1. Fare clic su Aggiungi.
2. Selezionare exercise1. master nella finestra di dialogo Seleziona una pagina master, come illustrato nella figura 7.
3. Fare clic su OK per aggiungere la pagina nuovo contenuto.

La pagina nuovo contenuto viene visualizzata in Visual Studio con un controllo contenuto per ogni controllo ContentPlaceHolder nella pagina master. Per impostazione predefinita, i controlli contenuto sono vuoti, in modo che sia possibile aggiungere contenuto personalizzato. Se si vuole che usino il contenuto del controllo ContentPlaceHolder nella pagina master, è sufficiente fare clic sul simbolo smart tag (la piccola freccia nera nell'angolo superiore destro del controllo) e scegliere *predefinito per padroneggiare il contenuto* dello smart tag, come illustrato nella **Figura 8**. Quando si esegue questa operazione, la voce di menu cambia per *creare contenuto personalizzato*. Facendo clic su di essa, il contenuto viene rimosso dalla pagina master, che consente di definire contenuto personalizzato per quel particolare controllo contenuto.

![Impostazione predefinita di un controllo contenuto sul contenuto delle pagine master](master-pages/_static/image4.gif)

**Figura 7**: impostazione predefinita di un controllo contenuto sul contenuto delle pagine master

## <a name="connecting-master-page-and-content-pages"></a>Connessione di pagina master e pagine di contenuto

L'associazione tra una pagina master e una pagina contenuto può essere configurata in uno dei quattro modi seguenti:

- Attributo <strong>MasterPageFile</strong> della direttiva @Page
- Impostazione della proprietà **Page. MasterPageFile** nel codice.
- Il **&lt;pagine&gt;** elemento nel file di configurazione delle applicazioni (Web. config nella cartella radice dell'applicazione)
- Il **&lt;pagine&gt;** elemento in un file di configurazione di sottocartelle (Web. config in una sottocartella)

## <a name="masterpagefile-attribute"></a>Attributo MasterPageFile

L'attributo MasterPageFile consente di applicare con facilità una pagina master a una pagina ASP.NET specifica. È anche il metodo usato per applicare la pagina master quando si seleziona la casella di controllo **Seleziona pagina master** come nell'esercizio 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Impostazione di Page. MasterPageFile nel codice

Impostando la proprietà MasterPageFile nel codice, è possibile applicare una particolare pagina master al contenuto in fase di esecuzione. Questa operazione è utile nei casi in cui potrebbe essere necessario applicare una pagina master specifica in base a un ruolo utente o ad altri criteri. La proprietà MasterPageFile deve essere impostata nel metodo PreInit. Se viene impostato dopo il metodo PreInit, verrà generata un'eccezione InvalidOperationException. La pagina in cui viene impostata questa proprietà deve avere anche un controllo contenuto come controllo di primo livello per la pagina. In caso contrario, verrà generata un'eccezione HttpException quando viene impostata la proprietà MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Utilizzo dell'elemento&gt; &lt;Pages

È possibile configurare una pagina master per le pagine impostando l'attributo masterPageFile nelle pagine &lt;&gt; elemento del file Web. config. Quando si usa questo metodo, tenere presente che i file Web. config più in basso nella struttura dell'applicazione possono eseguire l'override di questa impostazione. Anche tutti gli attributi MasterPageFile impostati in una direttiva @Page eseguiranno l'override di questa impostazione. L'utilizzo dell'elemento &lt;pagine&gt; semplifica la creazione *di una pagina master master* che può essere sottoposta a override, se necessario, in particolari cartelle o file.

## <a name="properties-in-master-pages"></a>Proprietà nelle pagine master

Una pagina master può esporre le proprietà semplicemente rendendo le proprietà pubbliche all'interno della pagina master. Ad esempio, il codice seguente definisce una proprietà denominata SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Per accedere alla proprietà SomeProperty dalla pagina contenuto, è necessario usare la proprietà master come indicato di seguito:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Annidamento di pagine master

Le pagine master sono la soluzione ideale per garantire un aspetto comune in un'applicazione Web di grandi dimensioni. Tuttavia, non è insolito che alcune parti di un sito di grandi dimensioni condividono un'interfaccia comune mentre altre parti condividono un'interfaccia diversa. Per rispondere a questa esigenza, più pagine master rappresentano la soluzione ideale. Tuttavia, ciò non riguarda ancora il fatto che un'applicazione di grandi dimensioni potrebbe avere determinati componenti, ad esempio un menu, che vengono condivisi tra tutte le pagine e altri componenti condivisi solo tra alcune sezioni del sito. Per questo tipo di situazione, le pagine master nidificate riempiono la necessità. Come si è visto, una pagina master normale è costituita da una pagina master e una pagina di contenuto. In una situazione di pagina master annidata sono presenti due pagine master; un master padre e un master figlio. La pagina master figlio è anche una pagina di contenuto e il relativo master è la pagina master padre.

Ecco il codice per una tipica pagina master:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

In uno scenario Master annidato, si tratta del master padre. In un'altra pagina master questa pagina verrà utilizzata come pagina master e il codice sarà simile al seguente:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Si noti che in questo scenario il master figlio è anche una pagina di contenuto per il master padre. Tutto il contenuto del master figlio viene visualizzato all'interno di un controllo contenuto che ne ottiene il contenuto dal controllo ContentPlaceHolder del padre.

> [!NOTE]
> Il supporto della finestra di progettazione non è disponibile per le pagine master nidificate. Quando si sviluppa utilizzando Master annidati, sarà necessario utilizzare la visualizzazione origine.

In questo video viene illustrata una procedura dettagliata relativa all'utilizzo di pagine master nidificate.

![](master-pages/_static/image1.png)

[Apri video a schermo intero](master-pages/_static/nested1.wmv)

![Selezione di una pagina master](master-pages/_static/image4.jpg)

**Figura 8**: selezione di una pagina master
