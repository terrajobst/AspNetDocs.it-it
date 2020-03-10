---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Controlli associati a dati | Microsoft Docs
author: microsoft
description: La maggior parte delle applicazioni ASP.NET si basa su un certo livello di presentazione dei dati da un'origine dati back-end. I controlli associati a dati sono stati una parte fondamentale dell'interazione tra w...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 1154b38e0fa3d9d56cb407ae659c3b0d69871fda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603838"
---
# <a name="data-bound-controls"></a>Controlli associati a dati

[Microsoft](https://github.com/microsoft)

> La maggior parte delle applicazioni ASP.NET si basa su un certo livello di presentazione dei dati da un'origine dati back-end. I controlli con associazione a dati sono stati una parte fondamentale dell'interazione con i dati nelle applicazioni Web dinamiche. ASP.NET 2,0 introduce alcuni miglioramenti sostanziali ai controlli con associazione a dati, tra cui una nuova classe BaseDataBoundControl e una sintassi dichiarativa.

La maggior parte delle applicazioni ASP.NET si basa su un certo livello di presentazione dei dati da un'origine dati back-end. I controlli con associazione a dati sono stati una parte fondamentale dell'interazione con i dati nelle applicazioni Web dinamiche. ASP.NET 2,0 introduce alcuni miglioramenti sostanziali ai controlli con associazione a dati, tra cui una nuova classe BaseDataBoundControl e una sintassi dichiarativa.

BaseDataBoundControl funge da classe base per la classe DataBoundControl e la classe HierarchicalDataBoundControl. In questo modulo verranno illustrate le classi seguenti che derivano da DataBoundControl:

- AdRotator
- Elenca controlli
- GridView
- FormView
- DetailsView

Verranno inoltre illustrate le classi seguenti che derivano dalla classe HierarchicalDataBoundControl:

- TreeView
- Menu
- SiteMapPath

## <a name="databoundcontrol-class"></a>Classe DataBoundControl

La classe DataBoundControl è una classe astratta (contrassegnata come MustInherit in VB) utilizzata per interagire con dati tabulari o di tipo elenco. I controlli seguenti sono alcuni dei controlli che derivano da DataBoundControl.

## <a name="adrotator"></a>AdRotator

Il controllo AdRotator consente di visualizzare un banner grafico in una pagina Web collegata a un URL specifico. Il grafico visualizzato viene ruotato usando le proprietà del controllo. La frequenza di un particolare annuncio visualizzato in una pagina può essere configurata usando la proprietà **Impressions** ed è possibile filtrare gli annunci usando il filtro delle parole chiave.

I controlli AdRotator utilizzano un file XML o una tabella in un database per i dati. Gli attributi seguenti vengono usati nei file XML per configurare il controllo AdRotator.

### <a name="imageurl"></a>ImageUrl
URL di un'immagine da visualizzare per l'annuncio.

### <a name="navigateurl"></a>NavigateUrl
URL a cui l'utente deve essere tenuto quando si fa clic su Active Directory. Deve essere codificato in URL.

### <a name="alternatetext"></a>AlternateText
Testo alternativo visualizzato in una descrizione comando e letto dalle utilità per la lettura dello schermo. Viene inoltre visualizzato quando l'immagine specificata da ImageUrl non è disponibile.

### <a name="keyword"></a>Parola chiave
Definisce una parola chiave che può essere usata quando si usa il filtro delle parole chiave. Se specificato, verranno visualizzati solo gli annunci con una parola chiave corrispondente al filtro parole chiave.

### <a name="impressions"></a>Impressioni
Numero di ponderazione che determina la frequenza con cui è probabile che venga visualizzato un determinato annuncio. È relativo all'impressione di altri annunci nello stesso file. Il valore massimo delle impressioni collettive per tutti gli annunci in un file XML è 2,048 miliardi 1.

### <a name="height"></a>Altezza
Altezza dell'annuncio in pixel.

### <a name="width"></a>Larghezza
Larghezza in pixel dell'annuncio.

> [!NOTE]
> Gli attributi Height e Width sostituiscono l'altezza e la larghezza del controllo AdRotator stesso.

Un file XML tipico potrebbe essere simile al seguente:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Nell'esempio precedente, l'annuncio per Contoso è il doppio della probabilità che venga visualizzato come ad per il sito Web ASP.NET a causa del valore dell'attributo Impressions.

Per visualizzare gli annunci dal file XML precedente, aggiungere un controllo AdRotator a una pagina e impostare la proprietà **proprietà AdvertisementFile** in modo che punti al file XML, come illustrato di seguito:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Se si sceglie di utilizzare una tabella di database come origine dati per il controllo AdRotator, sarà innanzitutto necessario configurare un database utilizzando lo schema seguente:

| **Nome colonna** | **Tipo di dati** | **Descrizione** |
| --- | --- | --- |
| ID | int | Chiave primaria. Questa colonna può avere qualsiasi nome. |
| ImageUrl | nvarchar (*length*) | URL relativo o assoluto dell'immagine da visualizzare per l'annuncio. |
| NavigateUrl | nvarchar (*length*) | URL di destinazione per l'annuncio. Se non si specifica un valore, l'annuncio non è un collegamento ipertestuale. |
| AlternateText | nvarchar (*length*) | Testo visualizzato se l'immagine non è stata trovata. In alcuni browser il testo viene visualizzato come descrizione comando. Il testo alternativo viene usato anche per l'accessibilità, in modo che gli utenti che non riescono a visualizzare il grafico possano ascoltare la propria descrizione. |
| Parola chiave | nvarchar (*length*) | Categoria per l'annuncio in cui la pagina può filtrare. |
| Impressioni | int(4) | Numero che indica la probabilità di frequenza con cui viene visualizzato l'annuncio. Maggiore è il numero, più spesso verrà visualizzato l'annuncio. Il totale di tutti i valori delle impressioni nel file XML non può superare 2,048 miliardi-1. |
| Larghezza | int(4) | Larghezza dell'immagine in pixel. |
| Altezza | int(4) | Altezza dell'immagine in pixel. |

Nei casi in cui si dispone già di un database con uno schema diverso, è possibile utilizzare le proprietà **AlternateTextField**, **ImageUrlField**e **NavigateUrlField** per eseguire il mapping degli attributi AdRotator al database esistente. Per visualizzare i dati dal database nel controllo AdRotator, aggiungere un controllo origine dati alla pagina, configurare la stringa di connessione per il controllo origine dati in modo che punti al database e impostare la proprietà **DataSourceID** del controllo AdRotator sull'ID del controllo origine dati. Nei casi in cui è necessario configurare gli annunci AdRotator a livello di codice, usare l'evento AdCreated. L'evento AdCreated accetta due parametri. un oggetto e l'altra un'istanza di AdCreatedEventArgs. AdCreatedEventArgs è un riferimento all'annuncio in fase di creazione.

Il frammento di codice seguente imposta l'oggetto ImageUrl, NavigateUrl e AlternateText per un annuncio a livello di codice:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Elenca controlli

I controlli elenco includono ListBox, DropDownList, CheckBoxList, RadioButtonList e BulletedList. Ognuno di questi controlli può essere associato a un'origine dei dati. Usano un campo nell'origine dati come testo visualizzato e possono facoltativamente usare un secondo campo come valore di un elemento. Gli elementi possono essere aggiunti anche in modo statico in fase di progettazione ed è possibile combinare elementi statici e elementi dinamici aggiunti da un'origine dati.

Per eseguire l'associazione dati di un controllo elenco, aggiungere un controllo origine dati alla pagina. Specificare un comando SELECT per il controllo origine dati, quindi impostare la proprietà DataSourceID del controllo elenco sull'ID del controllo origine dati. Usare le proprietà **TextField** e **DataValueField** per definire il testo visualizzato e il valore per il controllo. Inoltre, è possibile utilizzare la proprietà **DataTextFormatString** per controllare l'aspetto del testo visualizzato come segue:

| **Espressione** | **Descrizione** |
| --- | --- |
| Prezzo: {0:C} | Per i dati numerici/decimali. Visualizza il valore letterale "Price:" seguito da numeri nel formato valuta. Il formato della valuta dipende dalle impostazioni cultura specificate nell'attributo culture della direttiva **Page** o nel file Web. config. |
| {0:D4} | Per i dati di tipo Integer. Non può essere usato con numeri decimali. I numeri interi vengono visualizzati in un campo con riempimento zero che ha una larghezza di quattro caratteri. |
| {0:N2}% | Per i dati numerici. Consente di visualizzare il numero con precisione del punto 2 decimale seguito dal valore letterale "%". |
| {0:000.0} | Per i dati numerici/decimali. I numeri vengono arrotondati a una posizione decimale. I numeri composti da meno di tre cifre sono riempiti con degli zero. |
| {0:D} | Per i dati di data/ora. Visualizza il formato di data estesa ("giovedì, 06 agosto 1996"). Il formato della data dipende dalle impostazioni cultura della pagina o del file Web.config. |
| {0:d} | Per i dati di data/ora. Visualizza il formato di data breve ("12/31/99"). |
| {0:yy-MM-dd} | Per i dati di data/ora. Visualizza la data nel formato numerico anno-mese-giorno (96-08-06) |

## <a name="gridview"></a>GridView

Il controllo GridView consente la visualizzazione e la modifica dei dati tabulari usando un approccio dichiarativo ed è il successore del controllo DataGrid. Nel controllo GridView sono disponibili le funzionalità seguenti.

- Associazione ai controlli origine dati, ad esempio SqlDataSource.
- Funzionalità di ordinamento predefinite.
- Funzionalità di aggiornamento ed eliminazione predefinite.
- Funzionalità di paging predefinite.
- Funzionalità di selezione righe predefinite.
- Accesso programmatico al modello a oggetti GridView per impostare dinamicamente le proprietà, gestire gli eventi e così via.
- Più campi chiave.
- Più campi dati per le colonne collegamento ipertestuale.
- Aspetto personalizzabile tramite temi e stili.

**Campi colonna**

Ogni colonna nel controllo GridView è rappresentata da un oggetto DataControlField. Per impostazione predefinita, la proprietà AutoGenerateColumns è impostata su **true**per creare un oggetto AutoGeneratedField per ogni campo dell'origine dati. Ogni campo viene quindi sottoposto a rendering come colonna nel controllo GridView nell'ordine in cui ogni campo viene visualizzato nell'origine dati. È anche possibile controllare manualmente quali campi della colonna vengono visualizzati nel controllo **GridView** impostando la proprietà **AutoGenerateColumns** su **false** e quindi definendo una raccolta di campi di colonna personalizzata. Tipi di campi colonna diversi determinano il comportamento delle colonne nel controllo.

Nella tabella seguente sono elencati i diversi tipi di campi di colonna che è possibile utilizzare.

| **Tipo di campo colonna** | **Descrizione** |
| --- | --- |
| BoundField | Consente di visualizzare il valore di un campo in un'origine dati. Si tratta del tipo di colonna predefinito del controllo GridView. |
| ButtonField | Visualizza un pulsante di comando per ogni elemento nel controllo GridView. In questo modo è possibile creare una colonna di controlli Button personalizzati, ad esempio il pulsante Aggiungi o Rimuovi. |
| CheckBoxField | Visualizza una casella di controllo per ogni elemento nel controllo GridView. Questo tipo di campo di colonna viene comunemente usato per visualizzare i campi con un valore booleano. |
| CommandField | Visualizza i pulsanti di comando predefiniti per eseguire operazioni di selezione, modifica o eliminazione. |
| HyperLinkField | Visualizza il valore di un campo in un'origine dati come collegamento ipertestuale. Questo tipo di campo colonna consente di associare un secondo campo all'URL del collegamento ipertestuale. |
| ImageField | Visualizza un'immagine per ogni elemento nel controllo GridView. |
| TemplateField | Visualizza il contenuto definito dall'utente per ogni elemento nel controllo GridView in base a un modello specificato. Questo tipo di campo colonna consente di creare un campo colonna personalizzato. |

Per definire in modo dichiarativo una raccolta di campi colonna, aggiungere prima di tutto le **colonne&lt;** di apertura e chiusura&gt;tag tra i tag di apertura e di chiusura del controllo GridView. Quindi, elencare i campi di colonna che si desidera includere tra le **colonne&lt;** di apertura e di chiusura&gt;tag. Le colonne specificate vengono aggiunte alla raccolta Columns nell'ordine elencato. La raccolta **Columns** archivia tutti i campi colonna nel controllo e consente di gestire a livello di codice i campi colonna nel controllo GridView.

I campi colonna dichiarati in modo esplicito possono essere visualizzati in combinazione con i campi colonna generati automaticamente. Quando vengono utilizzati entrambi, viene eseguito prima il rendering dei campi di colonna dichiarati in modo esplicito, seguiti dai campi colonna generati automaticamente.

## <a name="binding-to-data"></a>Associazione ai dati

Il controllo GridView può essere associato a un controllo origine dati (ad esempio **SqlDataSource**, **ObjectDataSource**e così via), nonché a qualsiasi origine dati che implementi l'interfaccia System. Collections. IEnumerable (ad esempio System. Data. DataView, System. Collections. ArrayList o System. Collections. Hashtable). Per associare il controllo GridView al tipo di origine dati appropriato, utilizzare uno dei metodi seguenti:

- Per eseguire il binding a un controllo origine dati, impostare la proprietà DataSourceID del controllo GridView sul valore ID del controllo origine dati. Il controllo GridView viene automaticamente associato al controllo origine dati specificato e può sfruttare le funzionalità del controllo origine dati per eseguire operazioni di ordinamento, aggiornamento, eliminazione e paging. Si tratta del metodo preferito per l'associazione ai dati.
- Per eseguire l'associazione a un'origine dati che implementa l'interfaccia System. Collections. IEnumerable, impostare a livello di codice la proprietà DataSource del controllo GridView sull'origine dati, quindi chiamare il metodo DataBind. Quando si usa questo metodo, il controllo GridView non fornisce funzionalità di ordinamento, aggiornamento, eliminazione e paging predefinite. È necessario fornire questa funzionalità autonomamente.

## <a name="data-operations"></a>Operazioni sui dati

Il controllo GridView fornisce molte funzionalità predefinite che consentono all'utente di ordinare, aggiornare, eliminare, selezionare ed eseguire il paging degli elementi nel controllo. Quando il controllo GridView è associato a un controllo origine dati, il controllo GridView può sfruttare le funzionalità del controllo origine dati e fornire funzionalità di ordinamento, aggiornamento ed eliminazione automatiche.

> [!NOTE]
> Il controllo GridView può fornire supporto per l'ordinamento, l'aggiornamento e l'eliminazione con altri tipi di origini dati; Tuttavia, sarà necessario fornire un gestore eventi appropriato con l'implementazione per queste operazioni.

L'ordinamento consente all'utente di ordinare gli elementi nel controllo GridView rispetto a una colonna specifica facendo clic sull'intestazione della colonna. Per abilitare l'ordinamento, impostare la proprietà AllowSorting su **true**.

Le funzionalità di aggiornamento, eliminazione e selezione automatiche sono abilitate quando si fa clic su un pulsante in un campo colonna **ButtonField** o **TemplateField** , con il nome di comando "Edit", "Delete" e "Select", rispettivamente. Il controllo GridView può aggiungere automaticamente un campo colonna **CommandField** con un pulsante modifica, Elimina o seleziona se la proprietà AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateSelectButton è impostata su **true**, rispettivamente.

> [!NOTE]
> L'inserimento di record nell'origine dati non è supportato direttamente dal controllo GridView. Tuttavia, è possibile inserire record utilizzando il controllo GridView insieme al controllo DetailsView o FormView.

Anziché visualizzare contemporaneamente tutti i record nell'origine dati, il controllo GridView può suddividere automaticamente i record in pagine. Per abilitare il paging, impostare la proprietà AllowPaging su **true**.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo GridView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le diverse proprietà di stile.

| **Style (proprietà)** | **Descrizione** |
| --- | --- |
| AlternatingRowStyle | Impostazioni di stile per le righe di dati alternate nel controllo GridView. Quando questa proprietà è impostata, le righe di dati vengono visualizzate alternando tra le impostazioni RowStyle e **AlternatingRowStyle** . |
| EditRowStyle | Impostazioni di stile per la riga in corso di modifica nel controllo GridView. |
| EmptyDataRowStyle | Impostazioni di stile per la riga di dati vuota visualizzata nel controllo GridView quando l'origine dati non contiene record. |
| FooterStyle | Impostazioni di stile per la riga del piè di pagina del controllo GridView. |
| HeaderStyle | Impostazioni di stile per la riga di intestazione del controllo GridView. |
| PagerStyle | Impostazioni di stile per la riga di spostamento del controllo GridView. |
| RowStyle | Impostazioni di stile per le righe di dati nel controllo GridView. Quando viene impostata anche la proprietà **AlternatingRowStyle** , le righe di dati vengono visualizzate alternando le impostazioni **RowStyle** e **AlternatingRowStyle** . |
| SelectedRowStyle | Impostazioni di stile per la riga selezionata nel controllo GridView. |

È anche possibile visualizzare o nascondere parti diverse del controllo. Nella tabella seguente sono elencate le proprietà che controllano le parti visualizzate o nascoste.

| **Property** | **Descrizione** |
| --- | --- |
| ShowFooter | Consente di visualizzare o nascondere la sezione del piè di pagina del controllo GridView. |
| ShowHeader | Consente di visualizzare o nascondere la sezione di intestazione del controllo GridView. |

### <a name="events"></a>eventi

Il controllo GridView fornisce diversi eventi in base ai quali è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. Nella tabella seguente sono elencati gli eventi supportati dal controllo GridView.

| **Event** | **Descrizione** |
| --- | --- |
| PageIndexChanged | Si verifica quando si fa clic su uno dei pulsanti di spostamento, ma dopo che il controllo GridView gestisce l'operazione di paging. Questo evento viene comunemente usato quando è necessario eseguire un'attività dopo che l'utente passa a una pagina diversa del controllo. |
| PageIndexChanging | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma prima che il controllo GridView gestisca l'operazione di paging. Questo evento viene spesso usato per annullare l'operazione di paging. |
| RowCancelingEdit | Si verifica quando si fa clic sul pulsante Annulla di una riga, ma prima che il controllo GridView esca dalla modalità di modifica. Questo evento viene spesso usato per arrestare l'operazione di annullamento. |
| RowCommand | Si verifica quando si fa clic su un pulsante nel controllo GridView. Questo evento viene spesso usato per eseguire un'attività quando si fa clic su un pulsante nel controllo. |
| RowCreated | Si verifica quando viene creata una nuova riga nel controllo GridView. Questo evento viene spesso usato per modificare il contenuto di una riga quando viene creata la riga. |
| RowDataBound | Si verifica quando una riga di dati viene associata ai dati nel controllo GridView. Questo evento viene spesso usato per modificare il contenuto di una riga quando la riga è associata ai dati. |
| RowDeleted | Si verifica quando viene fatto clic sul pulsante Elimina di una riga, ma dopo che il controllo GridView ha eliminato il record dall'origine dati. Questo evento viene spesso usato per verificare i risultati dell'operazione di eliminazione. |
| RowDeleting | Si verifica quando viene fatto clic sul pulsante Elimina di una riga, ma prima che il controllo GridView elimini il record dall'origine dati. Questo evento viene spesso usato per annullare l'operazione di eliminazione. |
| RowEditing | Si verifica quando si fa clic sul pulsante modifica di una riga, ma prima che il controllo GridView entri in modalità di modifica. Questo evento viene spesso utilizzato per annullare l'operazione di modifica. |
| RowUpdated | Si verifica quando si fa clic sul pulsante Aggiorna di una riga, ma dopo che il controllo GridView ha aggiornato la riga. Questo evento viene spesso usato per verificare i risultati dell'operazione di aggiornamento. |
| RowUpdating | Si verifica quando si fa clic sul pulsante Aggiorna di una riga, ma prima che il controllo GridView aggiorni la riga. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| SelectedIndexChanged | Si verifica quando viene fatto clic sul pulsante Seleziona di una riga, ma dopo che il controllo GridView gestisce l'operazione di selezione. Questo evento viene spesso utilizzato per eseguire un'attività dopo che è stata selezionata una riga nel controllo. |
| SelectedIndexChanging | Si verifica quando viene fatto clic sul pulsante Seleziona di una riga, ma prima che il controllo GridView gestisca l'operazione di selezione. Questo evento viene spesso utilizzato per annullare l'operazione di selezione. |
| Ordinati | Si verifica quando si fa clic sul collegamento ipertestuale per l'ordinamento di una colonna, ma dopo che il controllo GridView gestisce l'operazione di ordinamento. Questo evento viene comunemente usato per eseguire un'attività dopo che l'utente fa clic su un collegamento ipertestuale per ordinare una colonna. |
| Ordinamento | Si verifica quando viene fatto clic sul collegamento ipertestuale per l'ordinamento di una colonna, ma prima che il controllo GridView gestisca l'operazione di ordinamento. Questo evento viene spesso utilizzato per annullare l'operazione di ordinamento o per eseguire una routine di ordinamento personalizzata. |

## <a name="formview"></a>FormView

Il controllo FormView viene utilizzato per visualizzare un singolo record da un'origine dati. È simile al controllo DetailsView, con la differenza che Visualizza i modelli definiti dall'utente anziché i campi riga. La creazione di modelli personalizzati garantisce una maggiore flessibilità nel controllo della modalità di visualizzazione dei dati. Il controllo FormView supporta le funzionalità seguenti:

- Associazione ai controlli origine dati, ad esempio SqlDataSource e ObjectDataSource.
- Funzionalità di inserimento predefinite.
- Funzionalità di aggiornamento ed eliminazione predefinite.
- Funzionalità di paging predefinite.
- Accesso programmatico al modello a oggetti FormView per impostare dinamicamente le proprietà, gestire gli eventi e così via.
- Aspetto personalizzabile tramite modelli, temi e stili definiti dall'utente.

## <a name="templates"></a>Modelli

Affinché il controllo FormView visualizzi il contenuto, è necessario creare modelli per le diverse parti del controllo. La maggior parte dei modelli è facoltativa. Tuttavia, è necessario creare un modello per la modalità in cui è configurato il controllo. Ad esempio, un controllo FormView che supporta l'inserimento di record deve disporre di un modello di elemento di inserimento definito. La tabella seguente elenca i diversi modelli che è possibile creare.

| **Tipo di modello** | **Descrizione** |
| --- | --- |
| EditItemTemplate | Definisce il contenuto per la riga di dati quando il controllo FormView è in modalità di modifica. Questo modello contiene in genere i controlli di input e i pulsanti di comando con cui l'utente può modificare un record esistente. |
| EmptyDataTemplate | Definisce il contenuto per la riga di dati vuota visualizzata quando il controllo FormView è associato a un'origine dati che non contiene record. Questo modello contiene in genere contenuto per avvisare l'utente che l'origine dati non contiene record. |
| FooterTemplate | Definisce il contenuto per la riga del piè di pagina. Questo modello contiene in genere eventuali contenuti aggiuntivi che si desidera visualizzare nella riga del piè di pagina. In alternativa, è possibile specificare semplicemente il testo da visualizzare nella riga del piè di pagina impostando la proprietà FooterText. |
| HeaderTemplate | Definisce il contenuto per la riga di intestazione. Questo modello contiene in genere eventuali contenuti aggiuntivi che si desidera visualizzare nella riga di intestazione. In alternativa, è possibile specificare semplicemente il testo da visualizzare nella riga di intestazione impostando la proprietà HeaderText. |
| ItemTemplate | Definisce il contenuto per la riga di dati quando il controllo FormView è in modalità di sola lettura. Questo modello contiene in genere contenuto per visualizzare i valori di un record esistente. |
| InsertItemTemplate | Definisce il contenuto per la riga di dati quando il controllo FormView è in modalità di inserimento. Questo modello contiene in genere i controlli di input e i pulsanti di comando con cui l'utente può aggiungere un nuovo record. |
| PagerTemplate | Definisce il contenuto per la riga di spostamento visualizzata quando la funzionalità di paging è abilitata (quando la proprietà AllowPaging è impostata su **true**). Questo modello contiene in genere i controlli con cui l'utente può passare a un altro record. |

È possibile associare i controlli di input nel modello di elemento di modifica e nel modello di elemento Insert ai campi di un'origine dati usando un'espressione di associazione bidirezionale. Ciò consente al controllo FormView di estrarre automaticamente i valori del controllo di input per un'operazione di aggiornamento o inserimento. Le espressioni di associazione bidirezionali consentono inoltre ai controlli di input in un modello di elemento di modifica di visualizzare automaticamente i valori dei campi originali.

### <a name="binding-to-data"></a>Associazione ai dati

Il controllo FormView può essere associato a un controllo origine dati (ad esempio **SqlDataSource**, AccessDataSource, **ObjectDataSource** e così via) o a qualsiasi origine dati che implementi l'interfaccia System. Collections. IEnumerable (ad esempio System. Data. DataView, System. Collections. ArrayList e System. Collections. Hashtable). Usare uno dei metodi seguenti per associare il controllo FormView al tipo di origine dati appropriato:

- Per eseguire il binding a un controllo origine dati, impostare la proprietà DataSourceID del controllo FormView sul valore ID del controllo origine dati. Il controllo FormView viene automaticamente associato al controllo origine dati specificato e può sfruttare le funzionalità del controllo origine dati per eseguire operazioni di inserimento, aggiornamento, eliminazione e paging. Si tratta del metodo preferito per l'associazione ai dati.
- Per eseguire l'associazione a un'origine dati che implementa l'interfaccia **System. Collections. IEnumerable** , impostare a livello di codice la proprietà DataSource del controllo FormView sull'origine dati, quindi chiamare il metodo DataBind. Quando si usa questo metodo, il controllo FormView non fornisce funzionalità predefinite di inserimento, aggiornamento, eliminazione e paging. È necessario fornire questa funzionalità usando l'evento appropriato.

## <a name="data-operations"></a>Operazioni sui dati

Il controllo FormView fornisce molte funzionalità predefinite che consentono all'utente di aggiornare, eliminare, inserire e scorrere gli elementi nel controllo. Quando il controllo FormView è associato a un controllo origine dati, il controllo FormView può sfruttare le funzionalità del controllo origine dati e fornire funzionalità automatiche di aggiornamento, eliminazione, inserimento e paging. Il controllo FormView può fornire supporto per operazioni di aggiornamento, eliminazione, inserimento e paging con altri tipi di origini dati. Tuttavia, è necessario fornire un gestore eventi appropriato con l'implementazione per queste operazioni.

Poiché il controllo FormView utilizza i modelli, non fornisce un modo per generare automaticamente i pulsanti di comando per eseguire operazioni di aggiornamento, eliminazione o inserimento. È necessario includere manualmente questi pulsanti di comando nel modello appropriato. Il controllo FormView riconosce determinati pulsanti le cui proprietà **CommandName** sono impostate su valori specifici. Nella tabella seguente sono elencati i pulsanti di comando riconosciuti dal controllo FormView.

| **Pulsante** | **Valore CommandName** | **Descrizione** |
| --- | --- | --- |
| Annulla | Annullare | Utilizzato nelle operazioni di aggiornamento o inserimento per annullare l'operazione e per rimuovere i valori immessi dall'utente. Il controllo FormView restituisce quindi alla modalità specificata dalla proprietà DefaultMode. |
| Eliminare | "Delete" | Utilizzato nelle operazioni di eliminazione per eliminare il record visualizzato dall'origine dati. Genera gli eventi ItemDeleting e ItemDeleted. |
| Edit | Modifica | Utilizzato nelle operazioni di aggiornamento per inserire il controllo FormView in modalità di modifica. Il contenuto specificato nella proprietà **EditItemTemplate** viene visualizzato per la riga di dati. |
| Inserisci | Inserire | Utilizzato nelle operazioni di inserimento per tentare di inserire un nuovo record nell'origine dati utilizzando i valori forniti dall'utente. Genera gli eventi ItemInserting e ItemInserted. |
| Nuovo | Nuovo | Utilizzato nelle operazioni di inserimento per inserire il controllo FormView in modalità di inserimento. Il contenuto specificato nella proprietà **InsertItemTemplate** viene visualizzato per la riga di dati. |
| Pagina | Pagina | Utilizzato nelle operazioni di paging per rappresentare un pulsante nella riga del pager che esegue il paging. Per specificare l'operazione di paging, impostare la proprietà **CommandArgument** del pulsante su "Next", "prec", "First", "Last" o l'indice della pagina in cui spostarsi. Genera gli eventi PageIndexChanging e PageIndexChanged. |
| Aggiorna | Aggiornamento | Utilizzato nelle operazioni di aggiornamento per tentare di aggiornare il record visualizzato nell'origine dati con i valori forniti dall'utente. Genera gli eventi ItemUpdating e ItemUpdated. |

Diversamente dal pulsante Elimina (che elimina immediatamente il record visualizzato), quando si fa clic sul pulsante modifica o nuovo, il controllo FormView passa rispettivamente alla modalità di modifica o inserimento. In modalità di modifica, il contenuto contenuto nella proprietà **EditItemTemplate** viene visualizzato per l'elemento di dati corrente. In genere, il modello di modifica dell'elemento viene definito in modo che il pulsante modifica venga sostituito da un aggiornamento e da un pulsante Annulla. Anche i controlli di input appropriati per il tipo di dati del campo, ad esempio un controllo TextBox o CheckBox, vengono in genere visualizzati con il valore di un campo che l'utente deve modificare. Se si fa clic sul pulsante Aggiorna, il record viene aggiornato nell'origine dati, mentre se si fa clic sul pulsante Annulla le modifiche vengono ignorate.

Analogamente, il contenuto contenuto nella proprietà **InsertItemTemplate** viene visualizzato per l'elemento di dati quando il controllo è in modalità di inserimento. Il modello di elemento Insert viene in genere definito in modo che il pulsante nuovo venga sostituito da un pulsante Insert e Cancel e che vengano visualizzati controlli di input vuoti che consentono all'utente di immettere i valori per il nuovo record. Se si fa clic sul pulsante Inserisci, il record viene inserito nell'origine dati, mentre se si fa clic sul pulsante Annulla le modifiche vengono ignorate.

Il controllo FormView fornisce una funzionalità di paging, che consente all'utente di passare ad altri record nell'origine dati. Se abilitata, viene visualizzata una riga di cercapersone nel controllo FormView che contiene i controlli di navigazione della pagina. Per abilitare il paging, impostare la proprietà **AllowPaging** su **true**. È possibile personalizzare la riga del pager impostando le proprietà degli oggetti contenuti nel pager e la proprietà PagerSettings. Anziché usare l'interfaccia utente della riga di spostamento incorporata, è possibile creare un'interfaccia utente personalizzata usando la proprietà **PagerTemplate** .

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo FormView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le diverse proprietà di stile.

| **Style (proprietà)** | **Descrizione** |
| --- | --- |
| EditRowStyle | Impostazioni di stile per la riga di dati quando il controllo FormView è in modalità di modifica. |
| EmptyDataRowStyle | Impostazioni di stile per la riga di dati vuota visualizzata nel controllo FormView quando l'origine dati non contiene record. |
| FooterStyle | Impostazioni di stile per la riga del piè di pagina del controllo FormView. |
| HeaderStyle | Impostazioni di stile per la riga di intestazione del controllo FormView. |
| InsertRowStyle | Impostazioni di stile per la riga di dati quando il controllo FormView è in modalità di inserimento. |
| PagerStyle | Impostazioni di stile per la riga di spostamento visualizzata nel controllo FormView quando la funzionalità di paging è abilitata. |
| RowStyle | Impostazioni di stile per la riga di dati quando il controllo FormView è in modalità di sola lettura. |

## <a name="events"></a>eventi

Il controllo FormView fornisce diversi eventi in base ai quali è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. Nella tabella seguente sono elencati gli eventi supportati dal controllo FormView.

| **Event** | **Descrizione** |
| --- | --- |
| ItemCommand | Si verifica quando si fa clic su un pulsante all'interno di un controllo FormView. Questo evento viene spesso usato per eseguire un'attività quando si fa clic su un pulsante nel controllo. |
| ItemCreated | Si verifica dopo la creazione di tutti gli oggetti FormViewRow nel controllo FormView. Questo evento viene spesso usato per modificare i valori di un record prima che venga visualizzato. |
| ItemDeleted | Si verifica quando si fa clic su un pulsante Elimina (un pulsante con la proprietà **CommandName** impostata su "Delete"), ma dopo che il controllo FormView ha eliminato il record dall'origine dati. Questo evento viene spesso usato per verificare i risultati dell'operazione di eliminazione. |
| ItemDeleting | Si verifica quando si fa clic su un pulsante Elimina, ma prima che il controllo FormView elimini il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| ItemInserted | Si verifica quando si fa clic su un pulsante Inserisci (un pulsante con la proprietà **CommandName** impostata su "Insert"), ma dopo che il controllo FormView inserisce il record. Questo evento viene spesso usato per verificare i risultati dell'operazione di inserimento. |
| ItemInserting | Si verifica quando viene fatto clic su un pulsante Inserisci, ma prima che il controllo FormView inserisca il record. Questo evento viene spesso utilizzato per annullare l'operazione di inserimento. |
| ItemUpdated | Si verifica quando si fa clic su un pulsante Aggiorna (un pulsante con la proprietà **CommandName** impostata su "Update"), ma dopo che il controllo FormView ha aggiornato la riga. Questo evento viene spesso usato per verificare i risultati dell'operazione di aggiornamento. |
| ItemUpdating | Si verifica quando si fa clic su un pulsante Aggiorna, ma prima che il controllo FormView aggiorni il record. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| ModeChanged | Si verifica dopo la modifica delle modalità del controllo FormView (per la modalità di modifica, inserimento o sola lettura). Questo evento viene spesso utilizzato per eseguire un'attività quando il controllo FormView modifica le modalità. |
| ModeChanging | Si verifica prima che il controllo FormView modifichi le modalità (per la modalità di modifica, inserimento o sola lettura). Questo evento viene spesso usato per annullare una modifica in modalità. |
| PageIndexChanged | Si verifica quando si fa clic su uno dei pulsanti di spostamento, ma dopo che il controllo FormView gestisce l'operazione di paging. Questo evento viene comunemente usato quando è necessario eseguire un'attività dopo che l'utente passa a un record diverso nel controllo. |
| PageIndexChanging | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma prima che il controllo FormView gestisca l'operazione di paging. Questo evento viene spesso usato per annullare l'operazione di paging. |

## <a name="detailsview"></a>DetailsView

Il controllo DetailsView viene utilizzato per visualizzare un singolo record da un'origine dati in una tabella, in cui ogni campo del record viene visualizzato in una riga della tabella. Può essere usato in combinazione con un controllo GridView per gli scenari Master-Detail. Il controllo DetailsView supporta le funzionalità seguenti:

- Associazione ai controlli origine dati, ad esempio SqlDataSource.
- Funzionalità di inserimento predefinite.
- Funzionalità di aggiornamento ed eliminazione predefinite.
- Funzionalità di paging predefinite.
- Accesso programmatico al modello a oggetti DetailsView per impostare dinamicamente le proprietà, gestire gli eventi e così via.
- Aspetto personalizzabile tramite temi e stili.

## <a name="row-fields"></a>Campi riga

Ogni riga di dati nel controllo DetailsView viene creata dichiarando un controllo campo. Tipi di campi riga diversi determinano il comportamento delle righe nel controllo. I controlli campo derivano da DataControlField. Nella tabella seguente sono elencati i diversi tipi di campi di riga che è possibile utilizzare.

| **Tipo di campo colonna** | **Descrizione** |
| --- | --- |
| BoundField | Visualizza il valore di un campo in un'origine dati come testo. |
| ButtonField | Visualizza un pulsante di comando nel controllo DetailsView. In questo modo è possibile visualizzare una riga con un controllo Button personalizzato, ad esempio un pulsante Aggiungi o Rimuovi. |
| CheckBoxField | Visualizza una casella di controllo nel controllo DetailsView. Questo tipo di campo di riga viene comunemente usato per visualizzare i campi con un valore booleano. |
| CommandField | Visualizza i pulsanti di comando predefiniti per eseguire operazioni di modifica, inserimento o eliminazione nel controllo DetailsView. |
| HyperLinkField | Visualizza il valore di un campo in un'origine dati come collegamento ipertestuale. Questo tipo di campo riga consente di associare un secondo campo all'URL del collegamento ipertestuale. |
| ImageField | Visualizza un'immagine nel controllo DetailsView. |
| TemplateField | Visualizza il contenuto definito dall'utente per una riga nel controllo DetailsView in base a un modello specificato. Questo tipo di campo riga consente di creare un campo riga personalizzato. |

Per impostazione predefinita, la proprietà AutoGenerateRows è impostata su **true**, che genera automaticamente un oggetto campo riga associato per ogni campo di un tipo associabile nell'origine dati. I tipi associabili validi sono String, DateTime, Decimal, GUID e il set di tipi primitivi. Ogni campo viene quindi visualizzato in una riga come testo, nell'ordine in cui ogni campo viene visualizzato nell'origine dati.

La generazione automatica delle righe consente di visualizzare in modo semplice e rapido ogni campo del record. Tuttavia, per sfruttare le funzionalità avanzate del controllo DetailsView, è necessario dichiarare in modo esplicito i campi riga da includere nel controllo DetailsView. Per dichiarare i campi riga, impostare innanzitutto la proprietà **AutoGenerateRows** su **false**. Successivamente, aggiungere i campi di apertura e chiusura **&lt;&gt;** tag tra i tag di apertura e di chiusura del controllo DetailsView. Infine, elencare i campi riga che si desidera includere tra i **campi&lt;** di apertura e di chiusura&gt;tag. I campi riga specificati vengono aggiunti alla raccolta Fields nell'ordine elencato. La raccolta **Fields** consente di gestire a livello di codice i campi riga nel controllo DetailsView.

> [!NOTE]
> I campi riga generati automaticamente non vengono aggiunti alla raccolta Fields.

## <a name="binding-to-data"></a>Associazione ai dati

Il controllo DetailsView può essere associato a un controllo origine dati, ad esempio **SqlDataSource** o AccessDataSource, oppure a qualsiasi origine dati che implementi l'interfaccia System. Collections. IEnumerable, ad esempio System. Data. DataView, System. Collections. ArrayList e System. Collections. Hashtable.

Per associare il controllo DetailsView al tipo di origine dati appropriato, utilizzare uno dei metodi seguenti:

- Per eseguire il binding a un controllo origine dati, impostare la proprietà DataSourceID del controllo DetailsView sul valore ID del controllo origine dati. Il controllo DetailsView viene associato automaticamente al controllo origine dati specificato. Si tratta del metodo preferito per l'associazione ai dati.
- Per eseguire l'associazione a un'origine dati che implementa l'interfaccia **System. Collections. IEnumerable** , impostare a livello di codice la proprietà DataSource del controllo DetailsView sull'origine dati, quindi chiamare il metodo DataBind.

## <a name="security"></a>Sicurezza

Questo controllo può essere usato per visualizzare l'input dell'utente, che potrebbe includere script client dannosi. Controllare le informazioni inviate da un client per uno script eseguibile, istruzioni SQL o altro codice prima di visualizzarlo nell'applicazione. ASP.NET fornisce una funzionalità di convalida della richiesta di input per bloccare lo script e il codice HTML nell'input dell'utente.

## <a name="data-operations"></a>Operazioni sui dati

Il controllo DetailsView fornisce funzionalità predefinite che consentono all'utente di aggiornare, eliminare, inserire e scorrere gli elementi nel controllo. Quando il controllo DetailsView è associato a un controllo origine dati, il controllo DetailsView può sfruttare le funzionalità del controllo origine dati e fornire funzionalità di aggiornamento, eliminazione, inserimento e paging automatiche.

Il controllo DetailsView può fornire supporto per operazioni di aggiornamento, eliminazione, inserimento e paging con altri tipi di origini dati. Tuttavia, è necessario fornire l'implementazione per queste operazioni in un gestore eventi appropriato.

Il controllo DetailsView può aggiungere automaticamente un campo di riga **CommandField** con un pulsante modifica, Elimina o nuovo impostando rispettivamente le proprietà AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateInsertButton su **true**. Diversamente dal pulsante Elimina (che elimina immediatamente il record selezionato), quando si fa clic sul pulsante modifica o nuovo, il controllo DetailsView passa rispettivamente alla modalità di modifica o inserimento. In modalità di modifica il pulsante modifica viene sostituito da un aggiornamento e da un pulsante Annulla. I controlli di input appropriati per il tipo di dati del campo, ad esempio un controllo TextBox o CheckBox, vengono visualizzati con il valore di un campo che l'utente deve modificare. Se si fa clic sul pulsante Aggiorna, il record viene aggiornato nell'origine dati, mentre se si fa clic sul pulsante Annulla le modifiche vengono ignorate. Analogamente, in modalità di inserimento, il pulsante nuovo viene sostituito con un pulsante di inserimento e un pulsante Annulla e vengono visualizzati controlli di input vuoti per l'utente per immettere i valori per il nuovo record.

Il controllo DetailsView fornisce una funzionalità di paging, che consente all'utente di passare ad altri record nell'origine dati. Se abilitati, i controlli per la navigazione tra le pagine vengono visualizzati in una riga di cercapersone. Per abilitare il paging, impostare la proprietà AllowPaging su **true**. La riga del pager può essere personalizzata usando le proprietà pager e PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo DetailsView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le diverse proprietà di stile.

| **Style (proprietà)** | **Descrizione** |
| --- | --- |
| AlternatingRowStyle | Impostazioni di stile per le righe di dati alternate nel controllo DetailsView. Quando questa proprietà è impostata, le righe di dati vengono visualizzate alternando tra le impostazioni RowStyle e **AlternatingRowStyle** . |
| CommandRowStyle | Impostazioni di stile per la riga contenente i pulsanti di comando incorporati nel controllo DetailsView. |
| EditRowStyle | Impostazioni di stile per le righe di dati quando il controllo DetailsView è in modalità di modifica. |
| EmptyDataRowStyle | Impostazioni di stile per la riga di dati vuota visualizzata nel controllo DetailsView quando l'origine dati non contiene record. |
| FooterStyle | Impostazioni di stile per la riga del piè di pagina del controllo DetailsView. |
| HeaderStyle | Impostazioni di stile per la riga di intestazione del controllo DetailsView. |
| InsertRowStyle | Impostazioni di stile per le righe di dati quando il controllo DetailsView è in modalità di inserimento. |
| PagerStyle | Impostazioni di stile per la riga di spostamento del controllo DetailsView. |
| RowStyle | Impostazioni di stile per le righe di dati nel controllo DetailsView. Quando viene impostata anche la proprietà **AlternatingRowStyle** , le righe di dati vengono visualizzate alternando le impostazioni **RowStyle** e **AlternatingRowStyle** . |
| FieldHeaderStyle | Impostazioni di stile per la colonna di intestazione del controllo DetailsView. |

## <a name="events"></a>eventi

Il controllo DetailsView fornisce diversi eventi in base ai quali è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. Nella tabella seguente sono elencati gli eventi supportati dal controllo DetailsView. Il controllo DetailsView eredita inoltre questi eventi dalle relative classi base: DataBinding, DataBound, eliminato, init, Load, PreRender e render.

| **Event** | **Descrizione** |
| --- | --- |
| ItemCommand | Si verifica quando si fa clic su un pulsante nel controllo DetailsView. |
| ItemCreated | Si verifica dopo la creazione di tutti gli oggetti DetailsViewRow nel controllo DetailsView. Questo evento viene spesso usato per modificare i valori di un record prima che venga visualizzato. |
| ItemDeleted | Si verifica quando si fa clic su un pulsante Elimina, ma dopo che il controllo DetailsView ha eliminato il record dall'origine dati. Questo evento viene spesso usato per verificare i risultati dell'operazione di eliminazione. |
| ItemDeleting | Si verifica quando si fa clic su un pulsante Elimina, ma prima che il controllo DetailsView elimini il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| ItemInserted | Si verifica quando si fa clic su un pulsante Inserisci, ma dopo che il controllo DetailsView inserisce il record. Questo evento viene spesso usato per verificare i risultati dell'operazione di inserimento. |
| ItemInserting | Si verifica quando si fa clic su un pulsante Inserisci, ma prima che il controllo DetailsView inserisca il record. Questo evento viene spesso utilizzato per annullare l'operazione di inserimento. |
| ItemUpdated | Si verifica quando si fa clic su un pulsante Aggiorna, ma dopo che il controllo DetailsView ha aggiornato la riga. Questo evento viene spesso usato per verificare i risultati dell'operazione di aggiornamento. |
| ItemUpdating | Si verifica quando si fa clic su un pulsante Aggiorna, ma prima che il controllo DetailsView aggiorni il record. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| ModeChanged | Si verifica dopo la modifica delle modalità del controllo DetailsView (modalità di modifica, inserimento o sola lettura). Questo evento viene spesso utilizzato per eseguire un'attività quando il controllo DetailsView cambia modalità. |
| ModeChanging | Si verifica prima che il controllo DetailsView modifichi le modalità (modifica, inserimento o modalità di sola lettura). Questo evento viene spesso usato per annullare una modifica in modalità. |
| PageIndexChanged | Si verifica quando si fa clic su uno dei pulsanti di spostamento, ma dopo che il controllo DetailsView gestisce l'operazione di paging. Questo evento viene comunemente usato quando è necessario eseguire un'attività dopo che l'utente passa a un record diverso nel controllo. |
| PageIndexChanging | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma prima che il controllo DetailsView gestisca l'operazione di paging. Questo evento viene spesso usato per annullare l'operazione di paging. |

## <a name="the-menu-control"></a>Controllo menu

Il controllo menu in ASP.NET 2,0 è progettato per essere un sistema di spostamento completo. È possibile associare i dati in modo semplice alle origini dati gerarchiche, ad esempio SiteMapDataSource.

Una struttura di controlli menu può essere definita in modo dichiarativo o dinamico ed è costituita da un singolo nodo radice e da un numero qualsiasi di sottonodi. Il codice seguente definisce in modo dichiarativo un menu per il controllo menu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Nell'esempio precedente, il nodo Home. aspx è il nodo radice. Tutti gli altri nodi sono annidati all'interno del nodo radice a vari livelli.

Sono disponibili due tipi di menu di cui è possibile eseguire il rendering nel controllo menu; menu statici e menu dinamici. I menu statici sono costituiti da voci di menu sempre visibili. I menu dinamici sono costituiti da voci di menu visibili solo quando l'utente passa il mouse su di essi. I clienti possono spesso confondere menu statici con menu definiti in modo dichiarativo e menu dinamici con i menu associati ai DataBinding in fase di esecuzione. In realtà, i menu dinamici e statici non sono correlati al metodo di popolamento. I termini *static* e *Dynamic* si riferiscono solo a se il menu viene o meno visualizzato per impostazione predefinita o se viene visualizzato solo quando l'utente esegue un'azione.

La proprietà **StaticDisplayLevels** viene utilizzata per configurare il numero di livelli del menu statici e pertanto visualizzati per impostazione predefinita. Nell'esempio precedente, se si imposta la proprietà **StaticDisplayLevels** su un valore pari a 2, il menu visualizzerà in modo statico il nodo Home, il nodo Music e il nodo Movies. Tutti gli altri nodi verranno visualizzati in modo dinamico quando l'utente passa il puntatore del mouse sul nodo padre.

La proprietà **MaximumDynamicDisplayLevels** configura il numero massimo di livelli dinamici che il menu è in grado di visualizzare. Eventuali menu dinamici a un livello superiore rispetto al valore specificato dalla proprietà **MaximumDynamicDisplayLevels** vengono eliminati.

> [!NOTE]
> È quasi certo che è possibile che si verifichino situazioni in cui i menu non sembrano eseguire il rendering a causa della proprietà MaximumDynamicDisplayLevels. In questi casi, assicurarsi che la proprietà sia impostata in modo sufficiente per consentire la visualizzazione dei menu Customers.

## <a name="data-binding-the-menu-control"></a>Associazione dati del controllo menu

Il controllo menu può essere associato a qualsiasi origine dati gerarchica, ad esempio SiteMapDataSource o XMLDataSource. SiteMapDataSource è il metodo più comunemente usato per data binding a un controllo menu perché si inserisce nel file Web. Sitemap e il relativo schema fornisce un'API nota al controllo menu. L'elenco seguente mostra un semplice file Web. sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Si noti che è presente un solo elemento siteMapNode radice, in questo caso l'elemento Home. È possibile configurare diversi attributi per ogni siteMapNode. Gli attributi usati più di frequente sono:

- **URL** di Specifica l'URL da visualizzare quando un utente fa clic sulla voce di menu. Se questo attributo non è presente, viene semplicemente eseguito il postback del nodo quando si fa clic su di esso.
- **titolo** Specifica il testo visualizzato nella voce di menu.
- **Descrizione** di Usato come documentazione per il nodo. Viene inoltre visualizzato come descrizione comando quando il mouse viene posizionato sul nodo.
- **siteMapFile** Consente le Sitemap nidificate. Questo attributo deve puntare a un file Sitemap ASP.NET ben formato.
- **ruoli** di Consente di controllare l'aspetto di un nodo mediante la rimozione di sicurezza ASP.NET.

Si noti che, sebbene questi attributi siano tutti facoltativi, il comportamento del menu potrebbe non essere quello previsto se non sono specificati. Se, ad esempio, l'attributo *URL* è specificato ma l'attributo *Description* non è, il nodo non sarà visibile e non sarà possibile passare all'URL specificato.

## <a name="controlling-a-menus-operation"></a>Controllo di un'operazione sui menu

Sono disponibili diverse proprietà che influiscono sul funzionamento di un controllo del menu ASP.NET. la proprietà **Orientation** , la proprietà **DisappearAfter** , la proprietà **StaticItemFormatString** e la proprietà **StaticPopOutImageUrl** sono solo alcune di queste.

- L' **orientamento** può essere impostato su *orizzontale* o *verticale* e controlla se le voci di menu statiche sono disposte orizzontalmente in una riga o verticalmente e sovrapposte l'una all'altra. Questa proprietà non influisce sui menu dinamici.
- La proprietà **DisappearAfter** configura per quanto tempo un menu dinamico deve rimanere visibile dopo che il mouse è stato rimosso. Il valore viene specificato in millisecondi e il valore predefinito è 500. Se si imposta questa proprietà su un valore pari a-1, il menu non scomparirà mai automaticamente. In tal caso, il menu scomparirà solo quando l'utente fa clic all'esterno del menu.
- La proprietà **StaticItemFormatString** consente di mantenere facilmente le verbosità coerenti nel sistema di menu. Quando si specifica questa proprietà, è necessario immettere *{0}* al posto della descrizione visualizzata nell'origine dati. Ad esempio, per fare in modo che la voce di menu dell'esercizio 1 includa visitare la pagina prodotti e così via, specificare visitare la pagina {0} per StaticItemFormatString. In fase di esecuzione, ASP.NET sostituirà tutte le occorrenze di {0} con la descrizione corretta per la voce di menu.
- La proprietà **StaticPopOutImageUrl** specifica l'immagine usata per indicare che un particolare nodo di menu dispone di nodi figlio a cui è possibile accedere passando il puntatore del mouse su di esso. I menu dinamici continueranno a usare l'immagine predefinita.

## <a name="templated-menu-controls"></a>Controlli menu basati su modelli

Il controllo menu è un controllo basato su modelli e consente due diversi ItemTemplate. StaticItemTemplate e DynamicItemTemplate. Usando questi modelli, è possibile aggiungere facilmente controlli server o controlli utente ai menu.

Per modificare i modelli in Visual Studio .NET, fare clic sul pulsante smart tag nel menu e scegliere modifica modelli. È quindi possibile scegliere di modificare StaticItemTemplate o DynamicItemTemplate.

Tutti i controlli aggiunti a StaticItemTemplate verranno visualizzati nel menu statico al caricamento della pagina. Tutti i controlli aggiunti a DynamicItemTemplate verranno visualizzati in tutti i menu a comparsa.

## <a name="menu-events"></a>Eventi menu

Il controllo menu è costituito da due eventi univoci. **MenuItemClicked** e l'evento **MenuItemDatabound** .

L'evento MenuItemClicked viene generato quando si fa clic su una voce di menu. L'evento MenuItemDatabound viene generato quando una voce di menu è associata ai DataBinding. Il **MenuEventArgs** passato al gestore eventi fornisce accesso alla voce di menu tramite la proprietà Item.

## <a name="controlling-a-menus-appearance"></a>Controllo dell'aspetto di un menu

È anche possibile influenzare l'aspetto di un controllo menu usando uno o più stili disponibili per formattare i menu. Tra queste sono **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**e **DynamicHoverStyle**. Queste proprietà vengono configurate utilizzando una stringa di stile HTML standard. Il codice seguente, ad esempio, avrà effetto sullo stile dei menu dinamici.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Se si usa uno degli stili del passaggio del mouse, sarà necessario aggiungere un elemento Head&gt; &lt;nella pagina con l'elemento *runat* impostato su *Server*.

I controlli menu supportano anche l'uso di temi ASP.NET 2,0.

## <a name="the-treeview-control"></a>Controllo TreeView

Il controllo TreeView Visualizza i dati in una struttura di tipo albero. Come nel caso del controllo menu, può essere facilmente associato a qualsiasi origine dati gerarchica, ad esempio SiteMapDataSource.

La prima domanda che i clienti potrebbero chiedere sul controllo TreeView in ASP.NET 2,0 è se è correlata o meno all'oggetto WebControl di IE di TreeView disponibile per ASP.NET 1. x. Non è. Il controllo TreeView ASP.NET 2,0 è stato scritto da zero e offre un miglioramento significativo rispetto al WebControl TreeView di IE disponibile in precedenza.

Non verrà illustrato in dettaglio come associare un controllo TreeView a una mappa del sito mentre viene eseguita esattamente come il controllo menu. Tuttavia, il controllo TreeView presenta alcune differenze distinte nel modo in cui opera.

Per impostazione predefinita, un controllo TreeView viene visualizzato completamente espanso. Per modificare il livello di espansione al caricamento iniziale, modificare la proprietà **ExpandDepth** del controllo. Questa operazione è particolarmente importante nei casi in cui TreeView è associato a dati quando si espandono nodi specifici.

## <a name="databinding-the-treeview-control"></a>Associazione di dati al controllo TreeView

A differenza del controllo menu, TreeView si presta a se stesso per la gestione di grandi quantità di dati. Oltre all'associazione dati a un controllo SiteMapDataSource o XMLDataSource, quindi, il controllo TreeView è spesso associato a un set di dati o ad altri dati relazionali. Nei casi in cui il controllo TreeView è associato a grandi quantità di dati, è preferibile associare solo ai dati effettivamente visibili nel controllo. È quindi possibile eseguire l'associazione dati a dati aggiuntivi mentre i nodi TreeView vengono espansi.

In questi casi, la proprietà **PopulateOnDemand** di TreeView deve essere impostata su *true*. Sarà quindi necessario fornire un'implementazione per il metodo **TreeNodePopulate** .

## <a name="data-binding-without-postback"></a>Data Binding senza PostBack

Si noti che quando si espande un nodo nell'esempio precedente per la prima volta, viene eseguito il postback della pagina e l'aggiornamento. In questo esempio non si tratta di un problema, ma è possibile immaginare che potrebbe trovarsi in un ambiente di produzione con una grande quantità di dati. Uno scenario migliore è quello in cui TreeView continuerà a popolare dinamicamente i nodi, ma senza postback al server.

Impostando le proprietà **PopulateNodesFromClient** e **PopulateOnDemand** su true, il controllo TreeView ASP.NET compilerà in modo dinamico i nodi senza eseguire il postback. Quando il nodo padre viene espanso, viene eseguita una richiesta XMLHTTP dal client e viene generato l'evento OnTreeNodePopulate. Il server risponde con un'isola di dati XML che viene quindi usata per associare i nodi figlio.

ASP.NET crea dinamicamente il codice client che implementa questa funzionalità. Lo script di &lt;&gt; Tag che contengono lo script viene generato puntando a un file AXD. Ad esempio, l'elenco seguente mostra i collegamenti script per il codice di script che genera la richiesta XMLHTTP.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Se si Esplora il file AXD sopra riportato nel browser e lo si apre, verrà visualizzato il codice che implementa la richiesta XMLHTTP. Questo metodo impedisce ai clienti di modificare il file di script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Controllo dell'operazione del controllo TreeView

Il controllo TreeView presenta diverse proprietà che influiscono sul funzionamento del controllo. Le proprietà più ovvie sono **ShowCheckBoxes**, **ShowExpandCollapse**e **ShowLines**.

La proprietà **ShowCheckBoxes** influiscono sull'eventuale visualizzazione di una casella di controllo da parte dei nodi durante il rendering. I valori validi per questa proprietà sono **None**, **root**, **Parent**, **Leaf**e **All**. Questi influiscono sul controllo TreeView come segue:

| **Valore proprietà** | **Effect** |
| --- | --- |
| Nessuna | Le caselle di controllo non vengono visualizzate in alcun nodo. Questa è l'impostazione predefinita. |
| Radice | Una casella di controllo viene visualizzata solo nel nodo radice. |
| Padre | Una casella di controllo viene visualizzata solo nei nodi che dispongono di nodi figlio. I nodi figlio possono essere nodi padre o nodi foglia. |
| Foglia | Una casella di controllo viene visualizzata solo nei nodi che non dispongono di nodi figlio. |
| All | Viene visualizzata una casella di controllo in tutti i nodi. |

Quando vengono usate le caselle di controllo, la proprietà **CheckedNodes** restituisce una raccolta di nodi TreeView che vengono controllati al postback.

La proprietà **ShowExpandCollapse** controlla l'aspetto dell'immagine di espansione/compressione accanto ai nodi radice e padre. Se questa proprietà è impostata su **false**, il rendering dei nodi TreeView viene eseguito come collegamenti ipertestuali e viene espanso o compresso facendo clic sul collegamento.

La proprietà **ShowLines** controlla se vengono visualizzate o meno le righe che connettono i nodi padre ai nodi figlio. Se **false** (impostazione predefinita), non viene visualizzata alcuna riga. Se è **true**, il controllo TreeView utilizzerà le immagini di linee nella cartella specificata dalla proprietà **LineImagesFolder** .

Per personalizzare l'aspetto delle righe di TreeView, Visual Studio .NET 2005 include uno strumento di progettazione linee. È possibile accedere a questo strumento utilizzando il pulsante smart tag del controllo TreeView come indicato di seguito.

![](data-bound-controls/_static/image1.jpg)

**Figura 1**

Quando si seleziona l'opzione di menu **Personalizza immagini linea** , viene avviato lo strumento Progettazione linee che consente di configurare l'aspetto delle righe di TreeView.

## <a name="treeview-events"></a>Eventi TreeView

Il controllo TreeView presenta gli eventi univoci seguenti:

- SelectedNodeChanged si verifica quando viene selezionato un nodo basato sulla proprietà **SelectAction** .
- TreeNodeCheckChanged si verifica quando viene modificato lo stato di una casella di controllo nodes.
- TreeNodeExpanded si verifica quando un nodo viene espanso in base alla proprietà **SelectAction** .
- TreeNodeCollapsed si verifica quando un nodo viene compresso.
- TreeNodeDataBound si verifica quando un nodo è associato a dati.
- TreeNodePopulate si verifica quando viene popolato un nodo.

La proprietà **SelectAction** consente di configurare l'evento che viene generato quando si seleziona un nodo. La proprietà SelectAction fornisce le azioni seguenti:

- TreeNodeSelectAction. Expand genera TreeNodeExpanded quando viene selezionato il nodo.
- TreeNodeSelectAction. None non genera alcun evento quando viene selezionato il nodo.
- TreeNodeSelectAction. Select genera l'evento SelectedNodeChanged quando viene selezionato il nodo.
- TreeNodeSelectAction. SelectExpand genera sia l'evento SelectedNodeChanged che l'evento TreeNodeExpanded quando viene selezionato il nodo.

## <a name="controlling-appearance-with-styles"></a>Controllo dell'aspetto con stili

Il controllo TreeView fornisce molte proprietà che consentono di controllare l'aspetto del controllo con gli stili. Sono disponibili le proprietà seguenti.

| **Nome proprietà** | **Controlli** |
| --- | --- |
| HoverNodeStyle | Controlla lo stile dei nodi quando il mouse viene spostato su di essi. |
| LeafNodeStyle | Controlla lo stile dei nodi foglia. |
| NodeStyle | Controlla lo stile per tutti i nodi. Gli stili specifici del nodo (ad esempio LeafNodeStyle) eseguono l'override di questo stile. |
| ParentNodeStyle | Controlla lo stile per tutti i nodi padre. |
| RootNodeStyle | Controlla lo stile del nodo radice. |
| SelectedNodeStyle | Consente di controllare lo stile del nodo selezionato. |

Ognuna di queste proprietà è di sola lettura. Tuttavia, restituiranno ogni oggetto **TreeNodeStyle** e le proprietà di tale oggetto possono essere modificate utilizzando il formato della *sottoproprietà Property* . Per impostare la proprietà **ForeColor** di **SelectedNodeStyle**, ad esempio, usare la sintassi seguente:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Si noti che il tag precedente non è chiuso. Questo perché quando si usa la sintassi dichiarativa illustrata di seguito, è necessario includere anche i nodi TreeView nel codice HTML.

È inoltre possibile specificare le proprietà di stile nel codice utilizzando il formato *Property. Subproperty* . Ad esempio, per impostare la proprietà **ForeColor** di **RootNodeStyle** nel codice, usare la sintassi seguente:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Per un elenco completo delle diverse proprietà di stile, vedere la documentazione MSDN sull'oggetto TreeNodeStyle.

## <a name="the-sitemappath-control"></a>Il controllo SiteMapPath

Il controllo SiteMapPath fornisce un controllo di navigazione con briciola di pane per gli sviluppatori ASP.NET. Analogamente agli altri controlli di navigazione, può essere facilmente associato a origini dati gerarchiche quali SiteMapDataSource o XmlDataSource.

Un controllo SiteMapPath è costituito da oggetti SiteMapNodeItem. Sono disponibili tre tipi di nodi: nodo radice, nodi padre e nodo corrente. Il nodo radice è il nodo nella parte superiore della struttura gerarchica. Il nodo corrente rappresenta la pagina corrente. Tutti gli altri nodi sono nodi padre.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Controllo dell'operazione del controllo SiteMapPath

Di seguito sono riportate le proprietà che controllano l'operazione del controllo SiteMapPath:

| **Property** | **Descrizione della proprietà** |
| --- | --- |
| ParentLevelsDisplayed | Controlla il numero di nodi padre visualizzati. Il valore predefinito è-1, che non impone alcuna restrizione sul numero di nodi padre visualizzati. |
| PathDirection | Controlla la direzione di SiteMapPath. I valori validi sono RootToCurrent (impostazione predefinita) e CurrentToRoot. |
| PathSeparator | Stringa che controlla il carattere che separa i nodi in un controllo SiteMapPath. Il valore predefinito è:. |
| RenderCurrentNodeAsLink | Valore booleano che controlla se il nodo corrente viene sottoposto a rendering come collegamento. Il valore predefinito è False. |
| SkipLinkText | Facilita l'accessibilità quando la pagina viene visualizzata dalle utilità per la lettura dello schermo. Questa proprietà consente agli utilità per la lettura dello schermo di ignorare il controllo SiteMapPath. Per disabilitare questa funzionalità, impostare la proprietà su String. Empty. |

## <a name="templated-sitemappath-controls"></a>Controlli SiteMapPath basati su modelli

SiteMapControl è un controllo basato su modelli e, di conseguenza, è possibile definire modelli diversi da utilizzare per la visualizzazione del controllo. Per modificare i modelli in un controllo SiteMapPath, fare clic sul pulsante smart tag del controllo e scegliere modifica modelli dal menu. Viene visualizzato il menu SiteMapTasks, come illustrato di seguito, in cui è possibile scegliere tra i diversi modelli disponibili.

![](data-bound-controls/_static/image2.jpg)

**Figura 2**

Il modello **NodeTemplate** fa riferimento a qualsiasi nodo in SiteMapPath. Se il nodo è un nodo radice o il nodo corrente e è configurato un **RootNodeTemplate** o **CurrentNodeTemplate** , viene eseguito l'override del NodeTemplate.

## <a name="sitemappath-events"></a>Eventi SiteMapPath

Il controllo SiteMapPath presenta due eventi che non sono derivati dalla classe del controllo. evento **ItemCreated** e evento **ItemDataBound** . L'evento ItemCreated viene generato quando viene creato un elemento SiteMapPath. ItemDataBound viene generato quando il metodo DataBind viene chiamato durante la data binding di un nodo SiteMapPath. Un oggetto **SiteMapNodeItemEventArgs** fornisce l'accesso al SiteMapNodeItem specifico tramite la proprietà Item.

## <a name="controlling-appearance-with-styles"></a>Controllo dell'aspetto con stili

Gli stili seguenti sono disponibili per la formattazione di un controllo SiteMapPath.

| **Nome proprietà** | **Controlli** |
| --- | --- |
| CurrentNodeStyle | Controlla lo stile del testo per il nodo corrente. |
| RootNodeStyle | Controlla lo stile del testo per il nodo radice. |
| NodeStyle | Controlla lo stile del testo per tutti i nodi presumendo che non venga applicato un CurrentNodeStyle o RootNodeStyle. |

La proprietà NodeStyle viene sottoposta a override da CurrentNodeStyle o RootNodeStyle. Ognuna di queste proprietà è di sola lettura e restituisce un oggetto di **stile** . Per influenzare l'aspetto di un nodo usando una di queste proprietà, è necessario impostare le proprietà dell'oggetto stile restituito. Il codice seguente, ad esempio, modifica la proprietà ForeColor del nodo corrente.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

La proprietà può essere applicata anche a livello di codice come segue:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Se viene applicato un modello, lo stile non verrà applicato.

## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Lab 1: configurazione di un controllo menu ASP.NET

1. Creare un nuovo sito Web.
2. Aggiungere un file della mappa del sito selezionando file, nuovo, file e scegliendo Mappa del sito dall'elenco di modelli di file.
3. Per impostazione predefinita, aprire la mappa del sito (Web. sitemap) e modificarla in modo che appaia come l'elenco seguente. Le pagine a cui si esegue il collegamento nel file della mappa del sito non esistono, ma ciò non costituisce un problema per questo esercizio.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Aprire il Web form predefinito in visualizzazione progettazione.
5. Dalla sezione navigazione della casella degli strumenti aggiungere un nuovo controllo menu alla pagina.
6. Dalla sezione dati della casella degli strumenti aggiungere un nuovo SiteMapDataSource. SiteMapDataSource utilizzerà automaticamente il file Web. Sitemap nel sito. Il file Web. sitemap *deve* trovarsi nella cartella radice del sito.
7. Fare clic sul controllo menu, quindi fare clic sul pulsante smart tag per visualizzare la finestra di dialogo attività di menu.
8. Nell'elenco a discesa Scegli origine dati selezionare SiteMapDataSource1.
9. Fare clic sul collegamento formattazione automatica e scegliere un formato per il menu.
10. Nel riquadro Proprietà impostare la proprietà **StaticDisplayLevels** su 2. Il controllo menu dovrebbe ora visualizzare il nodo Home, prodotti e servizi nella finestra di progettazione.
11. Esplorare la pagina nel browser per usare il menu. Poiché le pagine aggiunte alla mappa del sito non esistono effettivamente, verrà visualizzato un errore quando si tenta di passare a tali pagine.

Provare a modificare le proprietà StaticDisplayLevels e MaximumDynamicDisplayLevels per vedere come influiscono sulla modalità di rendering del menu.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Lab 2: associazione dinamica di un controllo TreeView

In questo esercizio si presuppone che sia stato eseguito SQL Server in locale e che il database Northwind sia presente nell'istanza di SQL Server. Se queste condizioni non vengono soddisfatte, modificare la stringa di connessione nell'esempio. Si noti che potrebbe essere necessario specificare SQL Server autenticazione anziché una connessione trusted.

1. Creare un nuovo sito Web.
2. Passa alla visualizzazione codice per default. aspx e Sostituisci tutto il codice con il codice riportato di seguito. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Salvare la pagina come TreeView. aspx.
4. Esplorare la pagina.
5. Quando la pagina viene visualizzata per la prima volta, visualizzare l'origine della pagina nel browser. Si noti che al client sono stati inviati solo i nodi visibili.
6. Fare clic sul segno più accanto a un nodo.
7. Visualizzare di nuovo l'origine nella pagina. Si noti che i nodi appena visualizzati sono ora presenti.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Lab 3: visualizzare i dettagli e modificare i dati usando GridView e DetailsView

1. Creare un nuovo sito Web.
2. Aggiungere un nuovo file Web. config al sito Web.
3. Aggiungere una stringa di connessione al file Web. config, come illustrato di seguito: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Potrebbe essere necessario modificare la stringa di connessione in base all'ambiente in uso.
4. Salvare e chiudere il file Web.config.
5. Aprire default. aspx e aggiungere un nuovo controllo SqlDataSource.
6. Modificare l'ID del controllo SqlDataSource in **Products**.
7. Nel menu **attività di SqlDataSource** fare clic su **Configura origine dati**.
8. Selezionare **Northwind** nell'elenco a discesa Connection (connessione) e fare clic su Avanti.
9. Selezionare **prodotti** nell'elenco a discesa **nome** e selezionare le caselle di controllo **ProductID**, **ProductName**, **PrezzoUnitario**e **UnitsInStock** come illustrato di seguito. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Fare clic su **Avanti**.
11. Scegliere **Fine**.
12. Passare alla visualizzazione origine ed esaminare il codice generato. Si notino **SelectCommand**, **DeleteCommand**, **InsertCommand**e **UpdateCommand** aggiunti al controllo SqlDataSource. Si notino anche i parametri aggiunti.
13. Passare alla visualizzazione progettazione e aggiungere un nuovo controllo GridView alla pagina.
14. Selezionare **prodotti** nell'elenco a discesa **Scegli origine dati** .
15. Selezionare **Abilita paging** e **Abilita selezione** come illustrato di seguito. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Fare clic sul collegamento **modifica colonne** e verificare che i **campi generazione automatica** siano selezionati.
17. Fare clic su **OK**.
18. Con il controllo GridView selezionato, fare clic sul pulsante accanto alla proprietà **al DataKeyNames** nel riquadro proprietà.
19. Selezionare **ProductID** dall'elenco **campi dati disponibili** e fare clic sul pulsante **&gt;** per aggiungerlo.
20. Fare clic su OK.
21. Aggiungere un nuovo controllo SqlDataSource alla pagina.
22. Modificare l'ID del controllo SqlDataSource in **Details**.
23. Dal menu attività di SqlDataSource scegliere **Configura origine dati**.
24. Scegliere **Northwind** dall'elenco a discesa e fare clic su **Avanti**.
25. Selezionare <strong>Products</strong> dall'elenco a discesa <strong>nome</strong> e selezionare la casella di controllo <strong>\</strong > * nella casella di riepilogo <strong>colonne</strong> .
26. Fare clic sul pulsante **where** .
27. Selezionare **ProductID** nell'elenco a discesa **colonna** .
28. Selezionare **=** nell'elenco a discesa operatore.
29. Selezionare il **controllo** dall'elenco a discesa **origine** .
30. Selezionare **GridView1** dall'elenco a discesa **ID controllo** .
31. Fare clic sul pulsante **Aggiungi** per aggiungere la clausola WHERE.
32. Fare clic su **OK**.
33. Fare clic sul pulsante **Avanzate** e selezionare la casella di controllo **genera istruzioni INSERT, Update e Delete** .
34. Fare clic su **OK**.
35. Fare clic su **Avanti** e fare clic su **fine**.
36. Aggiungere un controllo DetailsView alla pagina.
37. Nell'elenco a discesa **Scegli origine dati** scegliere **Dettagli**.
38. Selezionare la casella di controllo **Abilita modifica** come illustrato di seguito. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Salvare la pagina ed esplorare default. aspx.
40. Fare clic sul collegamento **Seleziona** accanto a record diversi per visualizzare automaticamente l'aggiornamento DetailsView.
41. Fare clic sul collegamento **modifica** nel controllo DetailsView.
42. Apportare una modifica al record e fare clic su **Aggiorna**.
