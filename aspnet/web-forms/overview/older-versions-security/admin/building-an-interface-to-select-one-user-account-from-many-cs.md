---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: Compilazione di un'interfaccia per la selezione di un account utenteC#da molti () | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà compilata un'interfaccia utente con una griglia filtrabile di paging. In particolare, l'interfaccia utente è costituita da una serie di LinkButton per...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 8057cfbcd33c74376076363bc27940cebd522c08
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575840"
---
# <a name="building-an-interface-to-select-one-user-account-from-many-c"></a>Creazione di un'interfaccia per la selezione di un account utente tra diversi account (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip) o [Scarica PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> In questa esercitazione verrà compilata un'interfaccia utente con una griglia filtrabile di paging. In particolare, l'interfaccia utente sarà costituita da una serie di LinkButton per filtrare i risultati in base alla lettera iniziale del nome utente e da un controllo GridView per visualizzare gli utenti corrispondenti. Si inizierà elencando tutti gli account utente in un controllo GridView. Quindi, nel passaggio 3, si aggiungeranno gli LinkButton del filtro. Il passaggio 4 esamina il paging dei risultati filtrati. L'interfaccia costruita nei passaggi da 2 a 4 verrà usata nelle esercitazioni successive per eseguire attività amministrative per un determinato account utente.

## <a name="introduction"></a>Introduzione

Nell'esercitazione <a id="_msoanchor_1"> </a> [*assegnazione dei ruoli agli utenti*](../roles/assigning-roles-to-users-cs.md) è stata creata un'interfaccia rudimentale che consente a un amministratore di selezionare un utente e gestirne i ruoli. In particolare, l'interfaccia ha presentato all'amministratore un elenco a discesa di tutti gli utenti. Un'interfaccia di questo tipo è utile quando sono presenti, a sua volta, un account utente, ma è difficile per i siti con centinaia o migliaia di account. Una griglia filtrabile di paging è un'interfaccia utente più adatta per i siti Web con basi utente di grandi dimensioni.

In questa esercitazione verrà compilata un'interfaccia utente di questo tipo. In particolare, l'interfaccia utente sarà costituita da una serie di LinkButton per filtrare i risultati in base alla lettera iniziale del nome utente e da un controllo GridView per visualizzare gli utenti corrispondenti. Si inizierà elencando tutti gli account utente in un controllo GridView. Quindi, nel passaggio 3, si aggiungeranno gli LinkButton del filtro. Il passaggio 4 esamina il paging dei risultati filtrati. L'interfaccia costruita nei passaggi da 2 a 4 verrà usata nelle esercitazioni successive per eseguire attività amministrative per un determinato account utente.

Iniziamo!

## <a name="step-1-adding-new-aspnet-pages"></a>Passaggio 1: aggiunta di nuove pagine di ASP.NET

In questa esercitazione e nei prossimi due verranno esaminate varie funzioni e funzionalità correlate all'amministrazione. Per implementare gli argomenti esaminati in queste esercitazioni, è necessaria una serie di pagine di ASP.NET. È ora di creare queste pagine e aggiornare la mappa del sito.

Per iniziare, creare una nuova cartella nel progetto denominata `Administration`. Aggiungere quindi due nuove pagine di ASP.NET alla cartella, collegando ogni pagina alla pagina master `Site.master`. Denominare le pagine:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Aggiungere anche due pagine alla directory radice del sito Web: `ChangePassword.aspx` e `RecoverPassword.aspx`.

A questo punto, queste quattro pagine hanno due controlli contenuto, uno per ogni ContentPlaceHolders della pagina master: `MainContent` e `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

Si desidera visualizzare il markup predefinito della pagina master per il `LoginContent` ContentPlaceHolder per queste pagine. Rimuovere quindi il markup dichiarativo per il controllo contenuto `Content2`. Al termine di questa operazione, il markup delle pagine deve contenere solo un controllo contenuto.

Le pagine ASP.NET della cartella `Administration` sono destinate esclusivamente agli utenti amministratori. È stato aggiunto un ruolo di amministratore al sistema nell' <a id="_msoanchor_2"> </a>esercitazione [*creazione e gestione dei ruoli*](../roles/creating-and-managing-roles-cs.md) . consente di limitare l'accesso a queste due pagine a questo ruolo. A tale scopo, aggiungere un file di `Web.config` alla cartella `Administration` e configurare il relativo elemento `<authorization>` per ammettere gli utenti nel ruolo Administrators e per negare tutti gli altri.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

A questo punto la Esplora soluzioni del progetto dovrebbe essere simile a quella mostrata nella figura 1.

[![quattro nuove pagine e un file Web. config sono stati aggiunti al sito Web](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**Figura 1**: sono state aggiunte quattro nuove pagine e un File di `Web.config` al sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))

Infine, aggiornare la mappa del sito (`Web.sitemap`) per includere una voce nella pagina `ManageUsers.aspx`. Aggiungere il codice XML seguente dopo il `<siteMapNode>` aggiunto per le esercitazioni sui ruoli.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

Con la mappa del sito aggiornata, visitare il sito tramite un browser. Come illustrato nella figura 2, la navigazione a sinistra include ora gli elementi per le esercitazioni sull'amministrazione.

[![la mappa del sito include un nodo denominato amministrazione utenti](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**Figura 2**: la mappa del sito include un nodo denominato amministrazione utenti ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))

## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Passaggio 2: elenco di tutti gli account utente in un controllo GridView

L'obiettivo finale di questa esercitazione è creare una griglia filtrabile di paging tramite la quale un amministratore può selezionare un account utente da gestire. Per iniziare, elencare *tutti* gli utenti in GridView. Una volta completata questa operazione, si aggiungeranno le interfacce e le funzionalità di filtro e paging.

Aprire la pagina `ManageUsers.aspx` nella cartella `Administration` e aggiungere un controllo GridView, impostando la relativa `ID` su `UserAccounts`. Nel momento in cui si scriverà il codice per associare il set di account utente a GridView usando il metodo `GetAllUsers` della classe `Membership`. Come illustrato nelle esercitazioni precedenti, il metodo GetAllUsers restituisce un oggetto `MembershipUserCollection`, ovvero una raccolta di oggetti `MembershipUser`. Ogni `MembershipUser` della raccolta include proprietà quali `UserName`, `Email`, `IsApproved`e così via.

Per visualizzare le informazioni sull'account utente desiderate in GridView, impostare la proprietà `AutoGenerateColumns` di GridView su false e aggiungere BoundField per le proprietà `UserName`, `Email`e `Comment` e CheckBoxFields per le proprietà `IsApproved`, `IsLockedOut`e `IsOnline`. Questa configurazione può essere applicata tramite il markup dichiarativo del controllo o tramite la finestra di dialogo campi. Nella figura 3 viene illustrata una schermata della finestra di dialogo campi dopo che la casella di controllo genera automaticamente campi è stata deselezionata e i BoundField e CheckBoxFields sono stati aggiunti e configurati.

[![aggiungere tre BoundField e tre CheckBoxFields al GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**Figura 3**: aggiungere tre BoundField e tre CheckBoxFields al GridView ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))

Dopo aver configurato GridView, verificare che il markup dichiarativo sia simile al seguente:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

Successivamente, è necessario scrivere il codice che associa gli account utente a GridView. Creare un metodo denominato `BindUserAccounts` per eseguire questa attività e quindi chiamarlo dal gestore dell'evento `Page_Load` nella prima pagina visita.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

È possibile eseguire il test della pagina tramite un browser. Come illustrato nella figura 4, il `UserAccounts` GridView elenca il nome utente, l'indirizzo di posta elettronica e altre informazioni sull'account pertinente per tutti gli utenti del sistema.

[![gli account utente sono elencati in GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**Figura 4**: gli account utente sono elencati in GridView ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))

## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Passaggio 3: filtrare i risultati in base alla prima lettera del nome utente

Attualmente il `UserAccounts` GridView Mostra *tutti* gli account utente. Per i siti Web con centinaia o migliaia di account utente, è essenziale che l'utente sia in grado di ridurre rapidamente gli account visualizzati. Questa operazione può essere eseguita aggiungendo filtri LinkButton alla pagina. Si aggiungono 27 LinkButton alla pagina: uno denominato tutti insieme a un LinkButton per ogni lettera dell'alfabeto. Se un visitatore fa clic su tutti gli LinkButton, GridView visualizzerà tutti gli utenti. Se si fa clic su una determinata lettera, verranno visualizzati solo gli utenti il cui nome utente inizia con la lettera selezionata.

La prima attività consiste nell'aggiungere i 27 controlli LinkButton. Un'opzione consiste nel creare i 27 LinkButton in modo dichiarativo, uno alla volta. Un approccio più flessibile consiste nell'utilizzare un controllo Repeater con un `ItemTemplate` che esegue il rendering di un LinkButton e quindi associa le opzioni di filtro al Repeater come matrice di `string`.

Per iniziare, aggiungere un controllo Repeater alla pagina sopra il `UserAccounts` GridView. Impostare la proprietà `ID` del Repeater su `FilteringUI`. Configurare i modelli del Repeater in modo che il relativo `ItemTemplate` esegua il rendering di un LinkButton le cui proprietà `Text` e `CommandName` sono associate all'elemento di matrice corrente. Come illustrato nell'esercitazione <a id="_msoanchor_3"> </a> [*assegnazione dei ruoli agli utenti*](../roles/assigning-roles-to-users-cs.md) , questa operazione può essere eseguita usando la sintassi di associazione dati `Container.DataItem`. Utilizzare la `SeparatorTemplate` del Repeater per visualizzare una linea verticale tra ogni collegamento.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

Per popolare questo Repeater con le opzioni di filtro desiderate, creare un metodo denominato `BindFilteringUI`. Assicurarsi di chiamare questo metodo dal gestore eventi `Page_Load` al primo caricamento della pagina.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

Questo metodo specifica le opzioni di filtro come elementi nella matrice di `string` `filterOptions`. Per ogni elemento nella matrice, il ripetitore eseguirà il rendering di un LinkButton con la relativa `Text` e `CommandName` proprietà assegnate al valore dell'elemento della matrice.

Nella figura 5 viene illustrata la pagina `ManageUsers.aspx` se visualizzata tramite un browser.

[![il Repeater elenca 27 i filtri LinkButton](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**Figura 5**: il Repeater elenca 27 controlli LinkButton di filtro ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))

> [!NOTE]
> I nomi utente possono iniziare con qualsiasi carattere, inclusi i numeri e la punteggiatura. Per visualizzare questi account, l'amministratore dovrà utilizzare l'opzione all LinkButton. In alternativa, è possibile aggiungere un LinkButton per restituire tutti gli account utente che iniziano con un numero. Lo lascio come esercizio per il lettore.

Se si fa clic su uno degli LinkButton di filtro, viene generato un postback e viene generato l'evento `ItemCommand` del ripetitore, ma non viene apportata alcuna modifica alla griglia perché è ancora necessario scrivere il codice per filtrare i risultati. La classe `Membership` include un [metodo`FindUsersByName`](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) che restituisce gli account utente il cui nome utente corrisponde a un criterio di ricerca specificato. È possibile utilizzare questo metodo per recuperare solo gli account utente i cui nomi utente iniziano con la lettera specificata dal `CommandName` del LinkButton filtrato selezionato.

Per iniziare, aggiornare la classe code-behind della pagina `ManageUser.aspx` in modo che includa una proprietà denominata `UsernameToMatch`. Questa proprietà mantiene la stringa del filtro nome utente nei postback:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

La proprietà `UsernameToMatch` archivia il valore che viene assegnato alla raccolta `ViewState` usando il valore UsernameToMatch della chiave. Quando il valore di questa proprietà viene letto, verifica se esiste un valore nella raccolta `ViewState`; in caso contrario, restituisce il valore predefinito, una stringa vuota. La proprietà `UsernameToMatch` presenta un modello comune, ovvero rende permanente un valore per visualizzare lo stato in modo che tutte le modifiche apportate alla proprietà vengano mantenute tra i postback. Per ulteriori informazioni su questo modello, vedere [informazioni sullo stato di visualizzazione ASP.NET](https://msdn.microsoft.com/library/ms972976.aspx).

Aggiornare quindi il metodo `BindUserAccounts` in modo che, invece di chiamare `Membership.GetAllUsers`, chiami `Membership.FindUsersByName`, passando il valore della proprietà `UsernameToMatch` accodato con il carattere jolly SQL%.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

Per visualizzare solo gli utenti il cui nome utente inizia con la lettera A, impostare la proprietà `UsernameToMatch` su un oggetto e quindi chiamare `BindUserAccounts`. Ciò comporta una chiamata a `Membership.FindUsersByName("A%")`, che restituirà tutti gli utenti il cui nome utente inizia con. Analogamente, per restituire *tutti* gli utenti, assegnare una stringa vuota alla proprietà `UsernameToMatch` in modo che il metodo `BindUserAccounts` richiamerà `Membership.FindUsersByName("%")`, restituendo quindi tutti gli account utente.

Creare un gestore eventi per l'evento `ItemCommand` del ripetitore. Questo evento viene generato ogni volta che si fa clic su uno degli LinkButton di filtro; viene passato il valore `CommandName` di LinkButton selezionato tramite l'oggetto `RepeaterCommandEventArgs`. È necessario assegnare il valore appropriato alla proprietà `UsernameToMatch` e quindi chiamare il metodo `BindUserAccounts`. Se il `CommandName` è all, assegnare una stringa vuota a `UsernameToMatch` in modo che tutti gli account utente vengano visualizzati. In caso contrario, assegnare il valore `CommandName` `UsernameToMatch`.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

Con questo codice, testare la funzionalità di filtro. Quando la pagina viene visitata per la prima volta, vengono visualizzati tutti gli account utente (fare riferimento alla figura 5). Se si fa clic su un LinkButton, viene visualizzato un postback e i risultati vengono filtrati, visualizzando solo gli account utente che iniziano con.

[![utilizzare gli LinkButton di filtro per visualizzare gli utenti il cui nome utente inizia con una determinata lettera](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**Figura 6**: usare gli LinkButton di filtro per visualizzare gli utenti il cui nome utente inizia con una determinata lettera ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))

## <a name="step-4-updating-the-gridview-to-use-paging"></a>Passaggio 4: aggiornamento di GridView per l'uso del paging

Il controllo GridView illustrato nelle figure 5 e 6 elenca tutti i record restituiti dal metodo `FindUsersByName`. Se sono presenti centinaia o migliaia di account utente, questo può comportare un sovraccarico di informazioni durante la visualizzazione di tutti gli account (come nel caso in cui si fa clic sul LinkButton tutti o quando inizialmente si visita la pagina). Per aiutare a presentare gli account utente in blocchi più gestibili, configurare GridView per visualizzare 10 account utente alla volta.

Il controllo GridView offre due tipi di paging:

- **Paging predefinito** -facile da implementare, ma inefficiente. In breve, con il paging predefinito, GridView prevede *tutti* i record dalla relativa origine dati. Viene quindi visualizzata solo la pagina di record appropriata.
- **Paging personalizzato** : richiede più lavoro da implementare, ma è più efficiente del paging predefinito perché con il paging personalizzato l'origine dati restituisce solo il set preciso di record da visualizzare.

La differenza di prestazioni tra il paging predefinito e quello personalizzato può essere piuttosto sostanziale durante il paging di migliaia di record. Poiché si sta compilando questa interfaccia presumendo che ci siano centinaia o migliaia di account utente, si userà il paging personalizzato.

> [!NOTE]
> Per una discussione più approfondita sulle differenze tra il paging predefinito e quello personalizzato, nonché i problemi correlati all'implementazione del paging personalizzato, vedere l'articolo relativo al [paging efficiente in grandi quantità di dati](https://asp.net/learn/data-access/tutorial-25-cs.aspx). Per un'analisi della differenza di prestazioni tra il paging predefinito e quello personalizzato, vedere [paging personalizzato in ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).

Per implementare il paging personalizzato, è necessario prima di tutto un meccanismo in base al quale recuperare il subset preciso di record visualizzati dal controllo GridView. La novità è che il metodo `FindUsersByName` della classe `Membership` dispone di un overload che consente di specificare l'indice e le dimensioni della pagina e restituisce solo gli account utente che rientrano in tale intervallo di record.

In particolare, questo overload ha la firma seguente: [`FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)`](https://msdn.microsoft.com/library/fa5st8b2.aspx).

Il parametro *pageIndex* specifica la pagina degli account utente da restituire; *pageSize* indica il numero di record da visualizzare per ogni pagina. Il parametro *totalRecords* è un parametro `out` che restituisce il numero totale di account utente nell'archivio utente.

> [!NOTE]
> I dati restituiti da `FindUsersByName` sono ordinati in base al nome utente; non è possibile personalizzare i criteri di ordinamento.

GridView può essere configurato in modo da utilizzare il paging personalizzato, ma solo quando è associato a un controllo ObjectDataSource. Affinché il controllo ObjectDataSource implementi il paging personalizzato, richiede due metodi: uno a cui viene passato un indice della riga iniziale e il numero massimo di record da visualizzare e restituisce il subset preciso di record che rientrano in tale intervallo; e un metodo che restituisce il numero totale di record di cui viene eseguito il paging. L'overload `FindUsersByName` accetta un indice e una dimensione di pagina e restituisce il numero totale di record tramite un parametro di `out`. Quindi, esiste un'interfaccia non corrispondente.

Un'opzione consiste nel creare una classe proxy che espone l'interfaccia prevista da ObjectDataSource, quindi chiama internamente il metodo `FindUsersByName`. Un'altra opzione, e quella da usare per questo articolo, consiste nel creare un'interfaccia di paging personalizzata e usarla invece dell'interfaccia di paging incorporata di GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Creazione di una prima, precedente, successiva, ultima interfaccia di paging

Verrà ora compilata un'interfaccia di paging con i primi, i precedenti, i successivi e gli ultimi LinkButton. Il primo LinkButton, quando si fa clic su di esso, porta l'utente alla prima pagina di dati, mentre l'operazione precedente lo restituirà alla pagina precedente. Analogamente, Next e Last sposteranno l'utente rispettivamente nella pagina successiva e nell'ultima pagina. Aggiungere i quattro controlli LinkButton sotto la `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

Successivamente, creare un gestore eventi per ogni evento di `Click` di LinkButton.

Nella figura 7 vengono illustrati i quattro LinkButton quando vengono visualizzati tramite la visualizzazione di progettazione di Visual Web Developer.

[![aggiungere LinkButton First, Previous, Next e Last sotto GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**Figura 7**: aggiungere LinkButton First, Previous, Next e Last sotto GridView ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))

### <a name="keeping-track-of-the-current-page-index"></a>Tenere traccia dell'indice della pagina corrente

Quando un utente visita la pagina `ManageUsers.aspx` o fa clic su uno dei pulsanti di filtro, si desidera visualizzare la prima pagina di dati nel controllo GridView. Quando l'utente fa clic su uno degli LinkButton di navigazione, tuttavia, è necessario aggiornare l'indice della pagina. Per mantenere l'indice di pagina e il numero di record da visualizzare per pagina, aggiungere le due proprietà seguenti alla classe code-behind della pagina:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

Analogamente alla proprietà `UsernameToMatch`, la proprietà `PageIndex` Salva in modo permanente il relativo valore per visualizzare lo stato. La proprietà di sola lettura `PageSize` restituisce un valore hardcoded, 10. Invito il lettore interessato ad aggiornare questa proprietà per usare lo stesso modello di `PageIndex`e quindi per aumentare la pagina `ManageUsers.aspx` in modo che la persona che visita la pagina possa specificare il numero di account utente da visualizzare per ogni pagina.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Recupero solo dei record della pagina corrente, aggiornamento dell'indice della pagina e abilitazione e disabilitazione degli LinkButton dell'interfaccia di paging

Con l'interfaccia di paging sul posto e le proprietà `PageIndex` e `PageSize` aggiunte, è possibile aggiornare il metodo `BindUserAccounts` in modo che usi l'overload di `FindUsersByName` appropriato. Inoltre, è necessario che questo metodo consenta di abilitare o disabilitare l'interfaccia di paging a seconda della pagina visualizzata. Quando si visualizza la prima pagina di dati, è necessario disabilitare il primo e il collegamento precedente; È necessario disabilitare Next e Last per visualizzare l'ultima pagina.

Aggiornare il metodo `BindUserAccounts` con il codice seguente:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

Si noti che il numero totale di record di cui viene eseguito il paging è determinato dall'ultimo parametro del metodo `FindUsersByName`. Si tratta di un parametro di `out`, quindi è necessario dichiarare prima una variabile per mantenere questo valore (`totalRecords`) e quindi anteporre alla parola chiave `out`.

Dopo che la pagina specificata degli account utente viene restituita, i quattro LinkButton sono abilitati o disabilitati, a seconda che venga visualizzata la prima o l'ultima pagina di dati.

L'ultimo passaggio consiste nel scrivere il codice per i quattro gestori di eventi `Click` di LinkButton. Questi gestori di eventi devono aggiornare la proprietà `PageIndex` e quindi riassociare i dati al GridView tramite una chiamata a `BindUserAccounts`. I gestori di eventi First, Previous e Next sono molto semplici. Il gestore dell'evento `Click` per l'ultimo LinkButton, tuttavia, è un po' più complesso perché è necessario determinare il numero di record visualizzati per determinare l'ultimo indice di pagina.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

Nelle figure 8 e 9 è illustrata l'interfaccia di paging personalizzata. Nella figura 8 viene illustrata la pagina `ManageUsers.aspx` quando si visualizza la prima pagina di dati per tutti gli account utente. Si noti che vengono visualizzati solo 10 dei 13 account. Facendo clic sul collegamento successivo o ultimo viene generato un postback, viene aggiornato il `PageIndex` a 1 e viene associata la seconda pagina degli account utente alla griglia (vedere la figura 9).

[![vengono visualizzati i primi 10 account utente](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**Figura 8**: vengono visualizzati i primi 10 account utente ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))

[![facendo clic sul collegamento successivo viene visualizzata la seconda pagina degli account utente](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**Figura 9**: facendo clic sul collegamento successivo viene visualizzata la seconda pagina di account utente ([fare clic per visualizzare l'immagine con dimensioni complete](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))

## <a name="summary"></a>Riepilogo

Gli amministratori devono spesso selezionare un utente dall'elenco di account. Nelle esercitazioni precedenti è stato esaminato l'uso di un elenco a discesa popolato con gli utenti, ma questo approccio non può essere ridimensionato correttamente. In questa esercitazione è stata illustrata un'alternativa migliore: un'interfaccia filtrabile i cui risultati vengono visualizzati in un GridView di paging. Con questa interfaccia utente, gli amministratori possono individuare e selezionare un account utente in modo rapido ed efficiente tra le migliaia.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Paging personalizzato in ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Paging efficiente in grandi quantità di dati](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [Implementazione dello strumento di amministrazione del sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore responsabile di questa esercitazione è Alicja Maziarz. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](recovering-and-changing-passwords-cs.md)
