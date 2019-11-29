---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
title: Pagine master annidateC#() | Microsoft Docs
author: rick-anderson
description: Viene illustrato come annidare una pagina master all'interno di un'altra.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 32b7fb6e-d74b-4048-91f8-70631b2523ee
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 67093266567a97cd22b353115616484fd9ef155e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74596780"
---
# <a name="nested-master-pages-c"></a>Pagine master annidate (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.pdf)

> Viene illustrato come annidare una pagina master all'interno di un'altra.

## <a name="introduction"></a>Introduzione

Nel corso delle ultime nove esercitazioni è stato illustrato come implementare un layout a livello di sito con le pagine master. In breve, le pagine master consentono a Microsoft, lo sviluppatore della pagina, di definire markup comune nella pagina master insieme a aree specifiche che possono essere personalizzate in base a una pagina di contenuto. I controlli ContentPlaceHolder in una pagina master indicano le aree personalizzabili; il markup personalizzato per i controlli ContentPlaceHolder è definito nella pagina contenuto tramite controlli contenuto.

Le tecniche della pagina master Esplorate finora sono molto utili se si dispone di un unico layout usato nell'intero sito. Tuttavia, molti siti Web di grandi dimensioni hanno un layout di sito personalizzato in varie sezioni. Si consideri ad esempio un'applicazione Health Care usata dal personale ospedaliero per gestire informazioni sui pazienti, attività e fatturazione. In questa applicazione possono essere presenti tre tipi di pagine Web:

- Pagine specifiche dei membri del personale in cui i membri del personale possono aggiornare la disponibilità, visualizzare le pianificazioni o richiedere il tempo di ferie.
- Pagine specifiche del paziente in cui i membri del personale visualizzano o modificano le informazioni per un paziente specifico.
- Pagine specifiche di fatturazione in cui i ragionieri esaminano gli Stati delle attestazioni correnti e i report finanziari.

Ogni pagina può condividere un layout comune, ad esempio un menu nella parte superiore e una serie di collegamenti di uso frequente lungo la parte inferiore. Tuttavia, le pagine specifiche per il personale, il paziente e la fatturazione potrebbero dover personalizzare il layout generico. Ad esempio, è possibile che tutte le pagine specifiche del personale includano un calendario e un elenco attività che mostra la disponibilità dell'utente attualmente connesso e la pianificazione giornaliera. Probabilmente è necessario che tutte le pagine specifiche del paziente mostrino il nome, l'indirizzo e le informazioni sulle assicurazioni per il paziente di cui vengono modificate le informazioni.

È possibile creare layout personalizzati con le *pagine master nidificate*. Per implementare lo scenario precedente, si inizia creando una pagina master che definisce il layout a livello di sito, il menu e il contenuto del piè di pagina, con ContentPlaceHolders che definiscono le aree personalizzabili. Si creeranno quindi tre pagine master annidate, una per ogni tipo di pagina Web. Ogni pagina master nidificata definisce il contenuto tra il tipo di pagine di contenuto che utilizzano la pagina master. In altre parole, la pagina master nidificata per le pagine di contenuto specifiche del paziente includerà il markup e la logica programmatica per la visualizzazione delle informazioni relative al paziente modificato. Quando si crea una nuova pagina specifica del paziente, la si associa a questa pagina master nidificata.

Questa esercitazione inizia con l'evidenziazione dei vantaggi delle pagine master nidificate. Viene quindi illustrato come creare e utilizzare le pagine master nidificate.

> [!NOTE]
> Le pagine master annidate sono state possibili dalla versione 2,0 del .NET Framework. Tuttavia, in Visual Studio 2005 non è stato incluso il supporto in fase di progettazione per le pagine master nidificate. La novità è che Visual Studio 2008 offre un'esperienza avanzata in fase di progettazione per le pagine master nidificate. Se si è interessati all'uso di pagine master annidate, ma si usa ancora Visual Studio 2005, consultare il post del Blog di [Scott Guthrie](https://weblogs.asp.net/scottgu/), [Suggerimenti per le pagine master nidificate in Visual Studio 2005 Design-Time](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).

## <a name="the-benefits-of-nested-master-pages"></a>Vantaggi delle pagine master annidate

Molti siti Web dispongono di una struttura di sito onnicomprensiva, oltre a progetti più personalizzati specifici di determinati tipi di pagine. Ad esempio, nell'applicazione Web demo è stata creata una sezione di amministrazione rudimentale, ovvero le pagine nella cartella `~/Admin`. Attualmente, le pagine Web nella cartella `~/Admin` utilizzano la stessa pagina master di quelle non presenti nella sezione amministrazione, ovvero `Site.master` o `Alternate.master`, a seconda della selezione dell'utente.

> [!NOTE]
> Per il momento, è possibile fingere che il sito disponga di una sola pagina master `Site.master`. Si utilizzeranno le pagine master annidate con due o più pagine master che iniziano con "utilizzo di una pagina master annidata per la sezione Amministrazione" più avanti in questa esercitazione.

Si supponga che sia stata richiesta la personalizzazione del layout delle pagine di amministrazione per includere informazioni aggiuntive o collegamenti che altrimenti non sarebbero presenti in altre pagine del sito. Sono disponibili quattro tecniche per implementare questo requisito:

1. Aggiungere manualmente le informazioni specifiche dell'amministrazione e i collegamenti a ogni pagina di contenuto nella cartella `~/Admin`.
2. Aggiornare la pagina master di `Site.master` per includere le informazioni e i collegamenti specifici della sezione di amministrazione, quindi aggiungere il codice alla pagina master per mostrare o nascondere queste sezioni a seconda che sia stata visitata una delle pagine di amministrazione.
3. Creare una nuova pagina master specificatamente per la sezione amministrazione, copiare il markup da `Site.master`, aggiungere le informazioni e i collegamenti specifici della sezione di amministrazione e quindi aggiornare le pagine di contenuto nella cartella `~/Admin` per usare questa nuova pagina master.
4. Creare una pagina master annidata associata a `Site.master` e fare in modo che le pagine di contenuto nella cartella `~/Admin` usino questa nuova pagina master nidificata. In questa pagina master nidificata sono incluse solo le informazioni aggiuntive e i collegamenti specifici per le pagine di amministrazione e non è necessario ripetere il markup già definito in `Site.master`.

La prima opzione è la meno appetibile. L'intero punto di utilizzo delle pagine master è quello di non dover copiare e incollare manualmente il markup comune nelle nuove pagine di ASP.NET. La seconda opzione è accettabile, ma rende l'applicazione meno gestibile poiché esegue il bulk delle pagine master con markup che viene visualizzato solo occasionalmente e richiede agli sviluppatori di modificare la pagina master per aggirare questo markup e ricordare quando, esattamente, viene visualizzato un certo markup rispetto al momento in cui è nascosto. Questo approccio è meno sostenibile come le personalizzazioni di più tipi di pagine Web che devono essere gestite da questa singola pagina master.

La terza opzione rimuove i problemi di disordine e complessità esposti con la seconda opzione. Tuttavia, lo svantaggio principale dell'opzione tre è che è necessario copiare e incollare il layout comune da `Site.master` alla nuova pagina master specifica della sezione amministrazione. Se successivamente si decide di modificare il layout a livello di sito, è necessario ricordarsi di modificarlo in due posizioni.

La quarta opzione, pagine master annidate, fornisce il meglio della seconda e della terza opzione. Le informazioni sul layout a livello di sito vengono mantenute in un unico file, ovvero la pagina master di primo livello, mentre il contenuto specifico per determinate aree viene separato in file diversi.

Questa esercitazione inizia con un'occhiata alla creazione e all'uso di una semplice pagina master nidificata. Viene creata una nuova pagina master di livello superiore, due pagine master annidate e due pagine di contenuto. A partire da "utilizzo di una pagina master annidata per la sezione Amministrazione", si esamina l'aggiornamento dell'architettura della pagina master esistente per includere l'utilizzo di pagine master nidificate. In particolare, viene creata una pagina master annidata che viene utilizzata per includere contenuto personalizzato aggiuntivo per le pagine di contenuto nella cartella `~/Admin`.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Passaggio 1: creazione di una semplice pagina master di primo livello

La creazione di un Master annidato basato su una delle pagine master esistenti e l'aggiornamento di una pagina di contenuto esistente per l'utilizzo della nuova pagina master nidificata anziché della pagina master di livello superiore comporta una certa complessità, perché le pagine di contenuto esistenti già prevedono determinate Controlli ContentPlaceHolder definiti nella pagina master di primo livello. La pagina master nidificata deve pertanto includere anche gli stessi controlli ContentPlaceHolder con gli stessi nomi. Inoltre, l'applicazione demo specifica dispone di due pagine master (`Site.master` e `Alternate.master`) che vengono assegnate dinamicamente a una pagina di contenuto in base alle preferenze dell'utente, che aumenta ulteriormente la complessità. Si esaminerà l'aggiornamento dell'applicazione esistente per l'utilizzo di pagine master nidificate più avanti in questa esercitazione, ma è necessario innanzitutto concentrarsi su un semplice esempio di pagine master nidificate.

Creare una nuova cartella denominata `NestedMasterPages`, quindi aggiungere un nuovo file di pagina master alla cartella denominata `Simple.master`. (Vedere la figura 1 per uno screenshot del Esplora soluzioni dopo l'aggiunta di questa cartella e file.) Trascinare il file del foglio di stile `AlternateStyles.css` dal Esplora soluzioni nella finestra di progettazione. Viene aggiunto un elemento `<link>` al file del foglio di stile nell'elemento `<head>`, dopo il quale il markup dell'elemento `<head>` della pagina master dovrebbe essere simile al seguente:

[!code-aspx[Main](nested-master-pages-cs/samples/sample1.aspx)]

Aggiungere quindi il markup seguente all'interno del Web Form di `Simple.master`:

[!code-aspx[Main](nested-master-pages-cs/samples/sample2.aspx)]

Questo markup Visualizza un collegamento denominato "pagine master annidate (semplici)" nella parte superiore della pagina in un tipo di carattere bianco grande su uno sfondo blu. Sotto che è il `MainContent` ContentPlaceHolder. Nella figura 1 viene illustrata la pagina master di `Simple.master` quando viene caricata nella finestra di progettazione di Visual Studio.

[![pagina master nidificata definisce il contenuto specifico per le pagine nella sezione Amministrazione](nested-master-pages-cs/_static/image2.png)](nested-master-pages-cs/_static/image1.png)

**Figura 01**: la pagina master nidificata definisce il contenuto specifico per le pagine nella sezione amministrazione ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image3.png))

## <a name="step-2-creating-a-simple-nested-master-page"></a>Passaggio 2: creazione di una semplice pagina master annidata

`Simple.master` contiene due controlli ContentPlaceHolder: il `MainContent` ContentPlaceHolder aggiunto all'interno del Web Form insieme al `head` ContentPlaceHolder nell'elemento `<head>`. Se dovessimo creare una pagina di contenuto e associarla a `Simple.master` la pagina contenuto avrebbe due controlli contenuto che fanno riferimento ai due ContentPlaceHolders. Analogamente, se si crea una pagina master annidata e la si associa a `Simple.master`, la pagina master nidificata avrà due controlli contenuto.

Aggiungere una nuova pagina master annidata alla cartella `NestedMasterPages` denominata `SimpleNested.master`. Fare clic con il pulsante destro del mouse sulla cartella `NestedMasterPages` e scegliere Aggiungi nuovo elemento. Viene visualizzata la finestra di dialogo Aggiungi nuovo elemento mostrata nella figura 2. Selezionare il tipo di modello di pagina master e digitare il nome della nuova pagina master. Per indicare che la nuova pagina master deve essere una pagina master annidata, selezionare la casella di controllo "Seleziona pagina master".

Fare quindi clic sul pulsante Aggiungi. Verrà visualizzata la stessa finestra di dialogo Seleziona una pagina master visualizzata quando si associa una pagina di contenuto a una pagina master (vedere la figura 3). Scegliere la pagina master `Simple.master` nella cartella `NestedMasterPages` e fare clic su OK.

> [!NOTE]
> Se il sito Web ASP.NET è stato creato usando il modello di progetto applicazione Web anziché il modello di progetto di sito Web, nella finestra di dialogo Aggiungi nuovo elemento non sarà visualizzata la casella di controllo "Seleziona pagina master" nella figura 2. Per creare una pagina master annidata quando si utilizza il modello di progetto applicazione Web, è necessario scegliere il modello di pagina master annidato anziché il modello di pagina master. Dopo aver selezionato il modello di pagina master annidato e aver fatto clic su Aggiungi, verrà visualizzata la stessa finestra di dialogo Seleziona una pagina master visualizzata nella figura 3.

[![selezionare la casella di controllo &quot;&quot; pagina master per aggiungere una pagina master annidata](nested-master-pages-cs/_static/image5.png)](nested-master-pages-cs/_static/image4.png)

**Figura 02**: selezionare la casella di controllo "Seleziona pagina master" per aggiungere una pagina master annidata ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image6.png))

[![associare la pagina master annidata alla pagina master semplice. master](nested-master-pages-cs/_static/image8.png)](nested-master-pages-cs/_static/image7.png)

**Figura 03**: associare la pagina master annidata alla pagina master di `Simple.master` ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image9.png))

Il markup dichiarativo della pagina master annidata, illustrato di seguito, contiene due controlli contenuto che fanno riferimento ai due controlli ContentPlaceHolder della pagina master di primo livello.

[!code-aspx[Main](nested-master-pages-cs/samples/sample3.aspx)]

Ad eccezione della direttiva `<%@ Master %>`, il markup dichiarativo iniziale della pagina master annidata è identico al markup generato inizialmente quando si associa una pagina di contenuto alla stessa pagina master di primo livello. Analogamente alla direttiva `<%@ Page %>` della pagina contenuto, la direttiva `<%@ Master %>` include un attributo `MasterPageFile` che specifica la pagina master padre della pagina master nidificata. La differenza principale tra la pagina master annidata e una pagina di contenuto associata alla stessa pagina master di primo livello è che la pagina master annidata può includere i controlli ContentPlaceHolder. I controlli ContentPlaceHolder della pagina master nidificata definiscono le aree in cui le pagine di contenuto possono personalizzare il markup.

Aggiornare la pagina master annidata in modo che visualizzi il testo "Hello, from SimpleNested!" nel controllo contenuto che corrisponde al controllo `MainContent` ContentPlaceHolder.

[!code-aspx[Main](nested-master-pages-cs/samples/sample4.aspx)]

Dopo aver apportato questa aggiunta, salvare la pagina master annidata e aggiungere una nuova pagina contenuto alla cartella `NestedMasterPages` denominata `Default.aspx`e associarla alla pagina master `SimpleNested.master`. Quando si aggiunge questa pagina, è possibile notare che non contiene controlli contenuto (vedere la figura 4). Una pagina contenuto può accedere solo al ContentPlaceHolders della pagina master *padre* . `SimpleNested.master` non contiene controlli ContentPlaceHolder; Pertanto, qualsiasi pagina di contenuto associata a questa pagina master non può contenere alcun controllo contenuto.

[![la nuova pagina contenuto non contiene controlli contenuto](nested-master-pages-cs/_static/image11.png)](nested-master-pages-cs/_static/image10.png)

**Figura 04**: la nuova pagina contenuto non contiene controlli contenuto ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image12.png))

È necessario aggiornare la pagina master annidata (`SimpleNested.master`) per includere i controlli ContentPlaceHolder. In genere, è necessario che le pagine master annidate includano un ContentPlaceHolder per ogni ContentPlaceHolder definito dalla relativa pagina master padre, consentendo in tal modo la pagina master figlio o la pagina di contenuto di funzionare con qualsiasi elemento ContentPlaceHolder della pagina master di primo livello. controlli.

Aggiornare la pagina master `SimpleNested.master` per includere un ContentPlaceHolder nei relativi due controlli contenuto. Assegnare a ContentPlaceHolder i controlli con lo stesso nome del controllo ContentPlaceHolder a cui si riferisce il controllo del contenuto. Ovvero, aggiungere un controllo ContentPlaceHolder denominato `MainContent` al controllo contenuto in `SimpleNested.master` che fa riferimento al `MainContent` ContentPlaceHolder in `Simple.master`. Eseguire la stessa operazione nel controllo contenuto che fa riferimento al `head` ContentPlaceHolder.

> [!NOTE]
> Sebbene sia consigliabile rinominare i controlli ContentPlaceHolder nella pagina master annidata come ContentPlaceHolders nella pagina master di primo livello, questa simmetria di denominazione non è obbligatoria. È possibile assegnare i controlli ContentPlaceHolder nella pagina master annidata come nome. Tuttavia, mi risulta più semplice ricordare quali ContentPlaceHolders corrispondono alle aree della pagina se la pagina master di primo livello e le pagine master nidificate utilizzano gli stessi nomi.

Dopo aver apportato queste aggiunte, il markup dichiarativo della pagina master di `SimpleNested.master` dovrebbe essere simile al seguente:

[!code-aspx[Main](nested-master-pages-cs/samples/sample5.aspx)]

Eliminare la pagina di contenuto `Default.aspx` appena creata e quindi aggiungerla nuovamente, associarla alla pagina master di `SimpleNested.master`. Questa volta in Visual Studio vengono aggiunti due controlli contenuto alla `Default.aspx`, che fanno riferimento al ContentPlaceHolders ora definito in `SimpleNested.master` (vedere la figura 6). Aggiungere il testo "Hello, from default. aspx!" nel controllo contenuto a cui si fa riferimento `MainContent`.

Nella figura 5 sono illustrate le tre entità che interessano `Simple.master`, `SimpleNested.master`e `Default.aspx` e il modo in cui si relazionano tra loro. Come illustrato nel diagramma, la pagina master annidata implementa i controlli contenuto per il relativo ContentPlaceHolder padre. Se queste aree devono essere accessibili alla pagina contenuto, la pagina master nidificata deve aggiungere il proprio ContentPlaceHolders ai controlli contenuto.

[![le pagine master nidificate e di primo livello stabiliscono il layout della pagina contenuto](nested-master-pages-cs/_static/image14.png)](nested-master-pages-cs/_static/image13.png)

**Figura 05**: le pagine master di primo livello e nidificate stabiliscono il layout della pagina di contenuto ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image15.png))

Questo comportamento illustra il modo in cui una pagina di contenuto o una pagina master è consapevole solo della pagina master padre. Questo comportamento è indicato anche dalla finestra di progettazione di Visual Studio. Nella figura 6 viene illustrata la finestra di progettazione per `Default.aspx`. Sebbene la finestra di progettazione mostri chiaramente le aree modificabili dalla pagina contenuto e le parti che non sono disponibili, non è in grado di distinguere le aree non modificabili dalla pagina master annidata e le aree geografiche della pagina master di primo livello.

[![la pagina contenuto include ora i controlli contenuto per il ContentPlaceHolders della pagina master annidata](nested-master-pages-cs/_static/image17.png)](nested-master-pages-cs/_static/image16.png)

**Figura 06**: la pagina contenuto include ora i controlli contenuto per il ContentPlaceHolders della pagina master annidata ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image18.png))

## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Passaggio 3: aggiunta di una seconda pagina master nidificata semplice

Il vantaggio delle pagine master nidificate è più evidente quando sono presenti più pagine master nidificate. Per illustrare questo vantaggio, creare un'altra pagina master annidata nella cartella `NestedMasterPages`; Denominare la nuova pagina master annidata `SimpleNestedAlternate.master` e associarla alla pagina master `Simple.master`. Aggiungere i controlli ContentPlaceHolder nei due controlli contenuto della pagina master annidata come nel passaggio 2. Aggiungere anche il testo "Hello, from SimpleNestedAlternate!" nel controllo contenuto che corrisponde al `MainContent` ContentPlaceHolder della pagina master di primo livello. Dopo aver apportato queste modifiche, il nuovo markup dichiarativo della pagina master nidificata avrà un aspetto simile al seguente:

[!code-aspx[Main](nested-master-pages-cs/samples/sample6.aspx)]

Creare una pagina di contenuto denominata `Alternate.aspx` nella cartella `NestedMasterPages` e associarla alla pagina master `SimpleNestedAlternate.master` nidificata. Aggiungere il testo "Hello, from alternate!" nel controllo contenuto che corrisponde a `MainContent`. La figura 7 Mostra `Alternate.aspx` quando viene visualizzata tramite la finestra di progettazione di Visual Studio.

[![alternativa. aspx è associato alla pagina master SimpleNestedAlternate. master](nested-master-pages-cs/_static/image20.png)](nested-master-pages-cs/_static/image19.png)

**Figura 07**: `Alternate.aspx` è associato alla pagina master `SimpleNestedAlternate.master` ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image21.png))

Confrontare la finestra di progettazione nella figura 7 con la finestra di progettazione nella figura 6. Entrambe le pagine di contenuto condividono lo stesso layout definito nella pagina master di primo livello (`Simple.master`), vale a dire il titolo "pagine master annidate (semplice)". In entrambe le pagine master padre, tuttavia, sono definiti contenuti distinti, ovvero il testo "Hello, from SimpleNested!" nella figura 6 e "Hello, from SimpleNestedAlternate!" nella figura 7. Queste differenze sono semplici, ma è possibile estendere questo esempio per includere differenze più significative. Ad esempio, la pagina `SimpleNested.master` potrebbe includere un menu con opzioni specifiche per le pagine di contenuto, mentre `SimpleNestedAlternate.master` potrebbero avere informazioni pertinenti per le pagine di contenuto associate.

A questo punto, si supponga di aver bisogno di apportare una modifica al layout del sito globale. Si supponga, ad esempio, di voler aggiungere un elenco di collegamenti comuni a tutte le pagine di contenuto. A tale scopo, si aggiorna la pagina master di primo livello, `Simple.master`. Tutte le modifiche apportate vengono immediatamente riflesse nelle pagine master annidate e, per estensione, le pagine di contenuto.

Per dimostrare la facilità con cui è possibile modificare il layout del sito che si desidera aggiungere, aprire la pagina master `Simple.master` e aggiungere il markup seguente tra gli elementi `topContent` e `mainContent` `<div>`:

[!code-aspx[Main](nested-master-pages-cs/samples/sample7.aspx)]

In questo modo vengono aggiunti due collegamenti alla parte superiore di ogni pagina associata a `Simple.master`, `SimpleNested.master`o `SimpleNestedAlternate.master`; Queste modifiche si applicano immediatamente a tutte le pagine master annidate e alle relative pagine di contenuto. Nella figura 8 viene illustrato `Alternate.aspx` quando viene visualizzato tramite un browser. Si noti l'aggiunta dei collegamenti nella parte superiore della pagina (rispetto alla figura 7).

[![modificate nella pagina master di primo livello vengono immediatamente riflesse nelle pagine master annidate e nelle relative pagine di contenuto](nested-master-pages-cs/_static/image23.png)](nested-master-pages-cs/_static/image22.png)

**Figura 08**: le modifiche apportate alla pagina master di primo livello vengono immediatamente riflesse nelle pagine master annidate e nelle pagine di contenuto ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image24.png))

## <a name="using-a-nested-master-page-for-the-administration-section"></a>Utilizzo di una pagina master nidificata per la sezione Amministrazione

A questo punto sono stati esaminati i vantaggi delle pagine master annidate e si è visto come crearli e usarli in un'applicazione ASP.NET. Gli esempi nei passaggi 1, 2 e 3, tuttavia, coinvolgono la creazione di una nuova pagina master di primo livello, nuove pagine master annidate e nuove pagine di contenuto. Che cosa accade per aggiungere una nuova pagina master annidata a un sito Web con una pagina master e pagine di contenuto di livello superiore esistenti?

L'integrazione di una pagina master annidata in un sito Web esistente e l'associazione con pagine di contenuto esistenti richiede un po' più di lavoro rispetto all'avvio da zero. I passaggi 4, 5, 6 e 7 consentono di esplorare questi problemi quando si aumenta l'applicazione demo in modo da includere una nuova pagina master annidata denominata `AdminNested.master` che contiene le istruzioni per l'amministratore e viene usata dalle pagine ASP.NET nella cartella `~/Admin`.

L'integrazione di una pagina master annidata nell'applicazione demo introduce gli ostacoli seguenti:

- Le pagine di contenuto esistenti nella cartella `~/Admin` hanno determinate aspettative dalla pagina master. Per i principianti, si aspettano che siano presenti determinati controlli ContentPlaceHolder. Inoltre, le pagine `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` chiamano il metodo di `RefreshRecentProductsGrid` pubblico della pagina master, ne impostano la proprietà `GridMessageText` o hanno un gestore eventi per l'evento `PricesDoubled`. Di conseguenza, la pagina master annidata deve fornire gli stessi membri ContentPlaceHolders e Public.
- Nell'esercitazione precedente è stata migliorata la classe `BasePage` per impostare dinamicamente la proprietà `MasterPageFile` dell'oggetto `Page` in base a una variabile di sessione. Come è possibile supportare le pagine master dinamiche quando si usano le pagine master annidate?

Questi due problemi emergono quando si compila la pagina master annidata e la si usa dalle pagine di contenuto esistenti. Questi problemi verranno analizzati e superati quando si presentano.

## <a name="step-4-creating-the-nested-master-page"></a>Passaggio 4: creazione della pagina master annidata

La prima attività consiste nel creare la pagina master annidata che verrà utilizzata dalle pagine della sezione amministrazione. Come illustrato nel passaggio 2, quando si aggiunge una nuova pagina master annidata è necessario specificare la pagina master padre della pagina master nidificata. Sono tuttavia disponibili due pagine master di primo livello: `Site.master` e `Alternate.master`. Si ricordi che è stato creato `Alternate.master` nell'esercitazione precedente e che è stato scritto il codice nella classe `BasePage` che imposta la proprietà `MasterPageFile` dell'oggetto pagina in fase di esecuzione su `Site.master` o `Alternate.master` a seconda del valore della variabile di sessione `MyMasterPage`.

Come si configura la pagina master annidata in modo da utilizzare la pagina master di primo livello appropriata? Sono disponibili due opzioni:

- Creare due pagine master annidate, `AdminNestedSite.master` e `AdminNestedAlternate.master`, e associarle rispettivamente alle pagine master di primo livello `Site.master` e `Alternate.master`. In `BasePage`, impostare il `MasterPageFile` dell'oggetto `Page` sulla pagina master annidata appropriata.
- Creare una singola pagina master annidata e fare in maniera che le pagine di contenuto usino questa particolare pagina master. Quindi, in fase di esecuzione, è necessario impostare la proprietà `MasterPageFile` della pagina master annidata sulla pagina master di primo livello appropriata in fase di esecuzione. Come è stato possibile intuire, le pagine master hanno anche una `MasterPageFile` proprietà).

Si userà la seconda opzione. Creare un singolo file di pagina master annidato nella cartella `~/Admin` denominata `AdminNested.master`. Poiché sia `Site.master` che `Alternate.master` hanno lo stesso set di controlli ContentPlaceHolder, non è importante la pagina master a cui è stato associato, anche se si consiglia di associarlo a `Site.master` per motivi di coerenza.

[![aggiungere una pagina master annidata alla cartella ~/admin](nested-master-pages-cs/_static/image26.png)](nested-master-pages-cs/_static/image25.png)

**Figura 09**: aggiungere una pagina master annidata alla cartella `~/Admin`. ([Fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image27.png))

Poiché la pagina master nidificata è associata a una pagina master con quattro controlli ContentPlaceHolder, Visual Studio aggiunge quattro controlli contenuto al nuovo markup iniziale del file della pagina master annidato. Analogamente ai passaggi 2 e 3, aggiungere un controllo ContentPlaceHolder in ogni controllo contenuto, assegnargli lo stesso nome del controllo ContentPlaceHolder della pagina master di primo livello. Aggiungere anche il markup seguente al controllo contenuto che corrisponde al `MainContent` ContentPlaceHolder:

[!code-html[Main](nested-master-pages-cs/samples/sample8.html)]

Definire quindi la classe CSS `instructions` nei file `Styles.css` e `AlternateStyles.css` CSS. Le seguenti regole CSS comportano la visualizzazione degli elementi HTML con lo stile della classe `instructions` con un colore di sfondo giallo chiaro e un bordo nero a tinta unita:

[!code-css[Main](nested-master-pages-cs/samples/sample9.css)]

Poiché questo markup è stato aggiunto alla pagina master nidificata, verrà visualizzato solo nelle pagine che utilizzano questa pagina master nidificata (ovvero le pagine della sezione Amministrazione).

Dopo aver apportato queste aggiunte alla pagina master annidata, il markup dichiarativo sarà simile al seguente:

[!code-aspx[Main](nested-master-pages-cs/samples/sample10.aspx)]

Si noti che ogni controllo contenuto dispone di un controllo ContentPlaceHolder e che ai controlli ContentPlaceHolder ' `ID` alle proprietà vengono assegnati gli stessi valori dei corrispondenti controlli ContentPlaceHolder nella pagina master di primo livello. Inoltre, il markup specifico della sezione di amministrazione viene visualizzato nel `MainContent` ContentPlaceHolder.

Nella figura 10 viene illustrata la `AdminNested.master` pagina master annidata quando viene visualizzata tramite la finestra di progettazione di Visual Studio. È possibile visualizzare le istruzioni nella casella gialla nella parte superiore del controllo del contenuto `MainContent`.

[![pagina master nidificata estende la pagina master di primo livello per includere le istruzioni per l'amministratore.](nested-master-pages-cs/_static/image29.png)](nested-master-pages-cs/_static/image28.png)

**Figura 10**: la pagina master nidificata estende la pagina master di primo livello per includere le istruzioni per l'amministratore. ([Fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image30.png))

## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Passaggio 5: aggiornamento delle pagine di contenuto esistenti per l'utilizzo della nuova pagina master annidata

Ogni volta che si aggiunge una nuova pagina di contenuto alla sezione amministrazione, è necessario associarla alla pagina master di `AdminNested.master` appena creata. Ma per quanto riguarda le pagine di contenuto esistenti? Attualmente, tutte le pagine di contenuto del sito derivano dalla classe `BasePage`, che imposta a livello di codice la pagina master della pagina contenuto in fase di esecuzione. Questo non è il comportamento che si desidera per le pagine di contenuto nella sezione amministrazione. Si vuole invece che queste pagine di contenuto usino sempre la pagina `AdminNested.master`. Sarà responsabilità della pagina master annidata scegliere la pagina di contenuto di livello principale corretta in fase di esecuzione.

Per ottenere questo comportamento desiderato, è possibile creare una nuova classe di pagina base personalizzata denominata `AdminBasePage` che estende la classe `BasePage`. `AdminBasePage` possibile eseguire l'override del `SetMasterPageFile` e impostare il `MasterPageFile` dell'oggetto `Page` sul valore hardcoded "~/Admin/AdminNested.master". In questo modo, tutte le pagine che derivano da `AdminBasePage` utilizzeranno `AdminNested.master`, mentre le pagine che derivano da `BasePage` avranno la proprietà `MasterPageFile` impostata dinamicamente su "~/site.master" o "~/alternate.master" in base al valore della variabile di sessione `MyMasterPage`.

Per iniziare, aggiungere un nuovo file di classe alla cartella `App_Code` denominata `AdminBasePage.cs`. `AdminBasePage` estendere `BasePage`, quindi eseguire l'override del metodo `SetMasterPageFile`. In tale metodo assegnare il `MasterPageFile` il valore "~/Admin/AdminNested.master". Dopo aver apportato queste modifiche, il file di classe avrà un aspetto simile al seguente:

[!code-csharp[Main](nested-master-pages-cs/samples/sample11.cs)]

È ora necessario che le pagine di contenuto esistenti nella sezione Amministrazione derivano da `AdminBasePage` anziché da `BasePage`. Passare al file di classe code-behind per ogni pagina di contenuto nella cartella `~/Admin` e apportare questa modifica. Ad esempio, in `~/Admin/Default.aspx` è necessario modificare la dichiarazione di classe code-behind da:

[!code-csharp[Main](nested-master-pages-cs/samples/sample12.cs)]

A:

[!code-csharp[Main](nested-master-pages-cs/samples/sample13.cs)]

Nella figura 11 viene illustrato il modo in cui la pagina master (`Site.master` o `Alternate.master`) di livello superiore, la pagina master annidata (`AdminNested.master`) e le pagine di contenuto della sezione di amministrazione sono correlate tra loro.

[![pagina master nidificata definisce il contenuto specifico per le pagine nella sezione Amministrazione](nested-master-pages-cs/_static/image32.png)](nested-master-pages-cs/_static/image31.png)

**Figura 11**: la pagina master nidificata definisce il contenuto specifico per le pagine nella sezione amministrazione ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image33.png))

## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Passaggio 6: mirroring delle proprietà e dei metodi pubblici della pagina master

Tenere presente che le pagine `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` interagiscono a livello di programmazione con la pagina master: `~/Admin/AddProduct.aspx` chiama il metodo pubblico `RefreshRecentProductsGrid` della pagina master e ne imposta la proprietà `GridMessageText`. `~/Admin/Products.aspx` dispone di un gestore eventi per l'evento `PricesDoubled`. Nell'esercitazione precedente è stata creata una classe abstract `BaseMasterPage` che ha definito questi membri pubblici.

Nelle pagine `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` si presuppone che la relativa pagina master derivi dalla classe `BaseMasterPage`. La pagina `AdminNested.master`, tuttavia, estende attualmente la classe `System.Web.UI.MasterPage`. Di conseguenza, quando si visita `~/Admin/Products.aspx` viene generata un'`InvalidCastException` con il messaggio: "Impossibile eseguire il cast dell'oggetto di tipo ' ASP. admin\_adminnested\_Master ' al tipo ' BaseMasterPage '".

Per risolvere questo problema, è necessario che la classe code-behind `AdminNested.master` si estenda `BaseMasterPage`. Aggiornare la dichiarazione di classe code-behind della pagina master annidata da:

[!code-csharp[Main](nested-master-pages-cs/samples/sample14.cs)]

A:

[!code-csharp[Main](nested-master-pages-cs/samples/sample15.cs)]

Non è ancora stato fatto. Poiché la classe `BaseMasterPage` è astratta, è necessario eseguire l'override dei membri `abstract`, `RefreshRecentProductsGrid` e `GridMessageText`. Questi membri vengono utilizzati dalle pagine master di primo livello per aggiornare le interfacce utente. In realtà, solo la pagina master di `Site.master` utilizza questi metodi, sebbene entrambe le pagine master di primo livello implementino questi metodi, dal momento che entrambe estendono `BaseMasterPage`.

Sebbene sia necessario implementare tali membri in `AdminNested.master`, tutte queste implementazioni devono semplicemente chiamare lo stesso membro nella pagina master di primo livello utilizzata dalla pagina master nidificata. Ad esempio, quando una pagina contenuto della sezione Amministrazione chiama il metodo `RefreshRecentProductsGrid` della pagina master annidata, è necessario che tutte le pagine master nidificate eseguano, a loro volta, la chiamata `Site.master` o `Alternate.master`il metodo `RefreshRecentProductsGrid`.

Per ottenere questo risultato, iniziare aggiungendo la direttiva di `@MasterType` seguente all'inizio del `AdminNested.master`:

[!code-aspx[Main](nested-master-pages-cs/samples/sample16.aspx)]

Si ricordi che la direttiva `@MasterType` aggiunge una proprietà fortemente tipizzata alla classe code-behind denominata `Master`. Eseguire quindi l'override dei membri `RefreshRecentProductsGrid` e `GridMessageText` e delegare semplicemente la chiamata al metodo corrispondente del `Master`:

[!code-csharp[Main](nested-master-pages-cs/samples/sample17.cs)]

Con questo codice, sarà possibile visitare e utilizzare le pagine di contenuto nella sezione amministrazione. Nella figura 12 viene illustrata la pagina `~/Admin/Products.aspx` se visualizzata tramite un browser. Come si può notare, la pagina include la casella istruzioni di amministrazione, definita nella pagina master nidificata.

[![le pagine di contenuto nella sezione amministrazione includono le istruzioni disponibili nella parte superiore di ogni pagina](nested-master-pages-cs/_static/image35.png)](nested-master-pages-cs/_static/image34.png)

**Figura 12**: le pagine di contenuto nella sezione amministrazione includono le istruzioni disponibili nella parte superiore di ogni pagina ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image36.png))

## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Passaggio 7: utilizzo della pagina master di primo livello appropriata in fase di esecuzione

Mentre tutte le pagine di contenuto nella sezione amministrazione sono completamente funzionanti, utilizzano tutte la stessa pagina master di primo livello e ignorano la pagina master selezionata dall'utente in `ChooseMasterPage.aspx`. Questo comportamento è dovuto al fatto che la proprietà `MasterPageFile` della pagina master annidata è impostata in modo statico su `Site.master` nella relativa direttiva `<%@ Master %>`.

Per usare la pagina master di primo livello selezionata dall'utente finale, è necessario impostare la proprietà `MasterPageFile` del `AdminNested.master`sul valore nella variabile di sessione `MyMasterPage`. Poiché le proprietà `MasterPageFile` delle pagine di contenuto sono state impostate in `BasePage`, si potrebbe pensare che la proprietà `MasterPageFile` della pagina master annidata venga impostata in `BaseMasterPage` o nella classe code-behind del `AdminNested.master`. Questo non funziona, tuttavia, perché è necessario impostare la proprietà `MasterPageFile` entro la fine della fase di PreInit. La prima fase in cui è possibile accedere a livello di codice al ciclo di vita della pagina da una pagina master è la fase Init (che si verifica dopo la fase di preinizializzazione).

Pertanto, è necessario impostare la proprietà `MasterPageFile` della pagina master annidata dalle pagine di contenuto. Le uniche pagine di contenuto che usano la `AdminNested.master` pagina master derivano da `AdminBasePage`. Quindi, possiamo inserire questa logica. Nel passaggio 5 è stato sovramontato il metodo `SetMasterPageFile`, impostando la proprietà `MasterPageFile` dell'oggetto `Page` su "~/Admin/AdminNested.master". Aggiornare `SetMasterPageFile` per impostare anche la proprietà `MasterPageFile` della pagina master sul risultato archiviato nella sessione:

[!code-csharp[Main](nested-master-pages-cs/samples/sample18.cs)]

Il metodo `GetMasterPageFileFromSession`, che è stato aggiunto alla classe `BasePage` nell'esercitazione precedente, restituisce il percorso del file della pagina master appropriato in base al valore della variabile di sessione.

Con questa modifica, la selezione della pagina master dell'utente viene riportata nella sezione amministrazione. La figura 13 Mostra la stessa pagina della figura 12, ma dopo che l'utente ha modificato la selezione della pagina master in `Alternate.master`.

[![pagina di amministrazione nidificata utilizza la pagina master di primo livello selezionata dall'utente](nested-master-pages-cs/_static/image38.png)](nested-master-pages-cs/_static/image37.png)

**Figura 13**: la pagina di amministrazione nidificata usa la pagina master di primo livello selezionata dall'utente ([fare clic per visualizzare l'immagine con dimensioni complete](nested-master-pages-cs/_static/image39.png))

## <a name="summary"></a>Riepilogo

Analogamente al modo in cui le pagine di contenuto possono essere associate a una pagina master, è possibile creare pagine master annidate mediante l'associazione di una pagina master figlio a una pagina master padre. La pagina master figlio può definire i controlli del contenuto per ogni ContentPlaceHolders padre. può quindi aggiungere i propri controlli ContentPlaceHolder (e altro markup) a questi controlli contenuto. Le pagine master annidate sono molto utili nelle applicazioni Web di grandi dimensioni, in cui tutte le pagine condividono un aspetto inarcabile, ma alcune sezioni del sito richiedono personalizzazioni univoche.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Pagine master ASP.NET annidate](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Suggerimenti per le pagine master annidate e la fase di progettazione di Visual Studio 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Visual Studio 2008 supporto della pagina master annidata](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 3,5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott può essere raggiunto in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](specifying-the-master-page-programmatically-cs.md)
> [Successivo](creating-a-site-wide-layout-using-master-pages-vb.md)
