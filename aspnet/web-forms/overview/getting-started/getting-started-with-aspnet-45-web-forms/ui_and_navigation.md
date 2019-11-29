---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interfaccia utente e navigazione | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: ac1dcaf1ba911fdcaeb3845c6836ec771733d93e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636810"
---
# <a name="ui-and-navigation"></a>Interfaccia utente ed esplorazione

di [Erik Reitan](https://github.com/Erikre)

[Scaricare il progetto di esempio WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare l'E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013 per il Web. Per accompagnare questa serie di esercitazioni è disponibile un [progetto Visual Studio 2013 C# con codice sorgente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) .

In questa esercitazione verrà modificata l'interfaccia utente dell'applicazione Web predefinita per supportare le funzionalità dell'applicazione front-end Toys Store Wingtip. Inoltre, si aggiungerà la navigazione semplice e con associazione a dati. Questa esercitazione si basa sull'esercitazione precedente "creare il livello di accesso ai dati" ed è parte della serie di esercitazioni su Wingtip Toys.

## <a name="what-youll-learn"></a>Cosa si apprenderà:

- Come modificare l'interfaccia utente per supportare le funzionalità dell'applicazione front Wingtip Toys Store.
- Come configurare un elemento HTML5 per includere l'esplorazione delle pagine.
- Come creare un controllo basato sui dati per passare a dati di prodotto specifici.
- Come visualizzare i dati da un database creato utilizzando Entity Framework Code First.

ASP.NET Web Forms consente di creare contenuto dinamico per l'applicazione Web. Ogni pagina Web ASP.NET viene creata in modo analogo a una pagina Web HTML statica, ovvero una pagina che non include l'elaborazione basata su server, ma la pagina Web ASP.NET include elementi aggiuntivi che ASP.NET riconosce e elabora per generare codice HTML quando viene eseguita la pagina.

Con una pagina HTML statica (file con*estensione htm* o *HTML* ), il server soddisfa una richiesta di `Web` leggendo il file e inviando il file così com'è al browser. Al contrario, quando un utente richiede una pagina Web ASP.NET (file con*estensione aspx* ), la pagina viene eseguita come programma sul server Web. Mentre la pagina è in esecuzione, è possibile eseguire qualsiasi attività richiesta dal sito Web, inclusi il calcolo dei valori, la lettura o la scrittura di informazioni del database o la chiamata di altri programmi. Come output, la pagina produce in modo dinamico il markup (ad esempio elementi in HTML) e invia questo output dinamico al browser.

## <a name="modifying-the-ui"></a>Modifica dell'interfaccia utente

Per continuare questa serie di esercitazioni, modificare la pagina *default. aspx* . Si modificherà l'interfaccia utente già stabilita dal modello predefinito utilizzato per creare l'applicazione. Il tipo di modifiche da eseguire è tipico quando si crea un'applicazione Web Form. Questa operazione viene eseguita modificando il titolo, sostituendo il contenuto e rimuovendo il contenuto predefinito non necessario.

1. Aprire o passare alla pagina *default. aspx* .
2. Se la pagina viene visualizzata nella visualizzazione **progettazione** , passare alla visualizzazione **origine** .
3. Nella parte superiore della pagina della direttiva `@Page` modificare l'attributo `Title` in "Welcome", come mostrato in giallo sotto. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Inoltre, nella pagina *default. aspx* sostituire tutto il contenuto predefinito contenuto nel tag `<asp:Content>` in modo che il markup appaia come indicato di seguito. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Salvare la pagina *default. aspx* selezionando **Save default. aspx** dal menu **file** .

   La pagina *default. aspx* risultante verrà visualizzata come segue: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

Nell'esempio è stato impostato l'attributo `Title` della direttiva `@Page`. Quando il codice HTML viene visualizzato in un browser, il codice del server `<%: Page.Title %>` viene risolto nel contenuto contenuto nell'attributo `Title`.

La pagina di esempio include gli elementi di base che costituiscono una pagina Web ASP.NET. La pagina contiene testo statico come si potrebbe avere in una pagina HTML, insieme a elementi specifici di ASP.NET. Il contenuto contenuto nella pagina *default. aspx* verrà integrato con il contenuto della pagina master, che verrà illustrato più avanti in questa esercitazione.

### <a name="page-directive"></a>@Page (direttiva)

I Web Form ASP.NET contengono in genere direttive che consentono di specificare le proprietà della pagina e le informazioni di configurazione per la pagina. Le direttive vengono utilizzate da ASP.NET come istruzioni per elaborare la pagina, ma non vengono visualizzate come parte del markup inviato al browser.

La direttiva utilizzata più di frequente è la direttiva `@Page`, che consente di specificare molte opzioni di configurazione per la pagina, incluse le seguenti:

1. Linguaggio di programmazione del server per il codice nella pagina, ad C#esempio.
2. Indica se la pagina è una pagina con codice server direttamente nella pagina, denominata pagina a file singolo, o se si tratta di una pagina con codice in un file di classe separato, denominato pagina code-behind.
3. Indica se la pagina è associata a una pagina master e pertanto deve essere considerata come una pagina di contenuto.
4. Opzioni di debug e di tracciatura.

Se non si include una direttiva `@Page` nella pagina o se la direttiva non include un'impostazione specifica, un'impostazione verrà ereditata dal file di configurazione *Web. config* o dal file di configurazione *Machine. config* . Il file *Machine. config* fornisce impostazioni di configurazione aggiuntive per tutte le applicazioni in esecuzione in un computer.

> [!NOTE] 
> 
> In *Machine. config* sono inoltre disponibili informazioni dettagliate su tutte le impostazioni di configurazione possibili.

### <a name="web-server-controls"></a>Controlli server Web

Nella maggior parte delle applicazioni Web Form ASP.NET, verranno aggiunti controlli che consentono all'utente di interagire con la pagina, ad esempio pulsanti, caselle di testo, elenchi e così via. Questi controlli server Web sono simili ai pulsanti HTML e agli elementi di input. Tuttavia, vengono elaborati nel server, consentendo di utilizzare il codice server per impostare le relative proprietà. Questi controlli generano inoltre eventi che è possibile gestire nel codice del server.

I controlli server utilizzano una sintassi speciale che ASP.NET riconosce quando viene eseguita la pagina. Il nome del tag per i controlli server ASP.NET inizia con un prefisso `asp:`. Questo consente a ASP.NET di riconoscere ed elaborare questi controlli server. Il prefisso può essere diverso se il controllo non fa parte del .NET Framework. Oltre al prefisso `asp:`, i controlli server ASP.NET includono anche l'attributo `runat="server"` e un `ID` che è possibile usare per fare riferimento al controllo nel codice del server.

Quando viene eseguita la pagina, ASP.NET identifica i controlli server ed esegue il codice associato a tali controlli. Molti controlli eseguono il rendering di un codice HTML o di un altro markup nella pagina quando viene visualizzato in un browser.

### <a name="server-code"></a>Codice server

La maggior parte delle applicazioni Web Form ASP.NET include il codice che viene eseguito nel server al momento dell'elaborazione della pagina. Come indicato in precedenza, il codice server può essere usato per eseguire una serie di operazioni, ad esempio l'aggiunta di dati a un controllo ListView. ASP.NET supporta l'esecuzione di molti linguaggi sul server, tra C#cui, Visual Basic, J# e altri.

ASP.NET supporta due modelli per la scrittura di codice server per una pagina Web. Nel modello a file singolo il codice per la pagina si trova in un elemento script in cui il tag di apertura include l'attributo `runat="server"`. In alternativa, è possibile creare il codice per la pagina in un file di classe separato, denominato modello code-behind. In questo caso, la pagina Web Form ASP.NET non contiene in genere codice server. Al contrario, la direttiva `@Page` include informazioni che collegano la pagina *aspx* al file code-behind associato.

L'attributo `CodeBehind` contenuto nella direttiva `@Page` specifica il nome del file di classe separato e l'attributo `Inherits` specifica il nome della classe all'interno del file code-behind che corrisponde alla pagina.

### <a name="updating-the-master-page"></a>Aggiornamento della pagina master

In ASP.NET Web Forms le pagine master consentono di creare un layout coerente per le pagine dell'applicazione. Una singola pagina master definisce l'aspetto e il comportamento standard desiderati per tutte le pagine (o un gruppo di pagine) nell'applicazione. È quindi possibile creare singole pagine di contenuto che contengono il contenuto che si desidera visualizzare, come illustrato in precedenza. Quando gli utenti richiedono le pagine di contenuto, ASP.NET le unisce con la pagina master per produrre l'output che combina il layout della pagina master con il contenuto della pagina di contenuto.

Il nuovo sito necessita di un singolo logo da visualizzare in ogni pagina. Per aggiungere questo logo, è possibile modificare il codice HTML nella pagina master.

1. In **Esplora soluzioni**individuare e aprire la pagina **site. master** .
2. Se la pagina è in visualizzazione **progettazione** , passare alla visualizzazione **origine** .
3. Aggiornare la pagina master **modificando o aggiungendo** il markup evidenziato in giallo: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

In questo codice HTML verrà visualizzata l'immagine denominata *logo. jpg* dalla cartella *Immagini* dell'applicazione Web, che verrà aggiunta in un secondo momento. Quando una pagina che utilizza la pagina master viene visualizzata in un browser, il logo viene visualizzato. Se un utente fa clic sul logo, l'utente si sposta nuovamente nella pagina *default. aspx* . Il tag di ancoraggio HTML `<a>` esegue il wrapping del controllo server immagini e consente di includere l'immagine come parte del collegamento. L'attributo `href` per il tag anchor specifica il "`~/`" radice del sito Web come percorso del collegamento. Per impostazione predefinita, la pagina *default. aspx* viene visualizzata quando l'utente passa alla radice del sito Web. L' **immagine** `<asp:Image>` controllo server include proprietà aggiuntive, ad esempio `BorderStyle`, che eseguono il rendering come HTML quando vengono visualizzate in un browser.

### <a name="master-pages"></a>Pagine master

Una pagina master è un file ASP.NET con estensione master (ad esempio, *site. master*) con un layout predefinito che può includere testo statico, elementi HTML e controlli server. La pagina master viene identificata da una specifica `@Master` direttiva che sostituisce la direttiva `@Page` utilizzata per le pagine comuni *. aspx* .

Oltre alla direttiva `@Master`, la pagina master contiene anche tutti gli elementi HTML di primo livello per una pagina, ad esempio `html`, `head`e `form`. Ad esempio, nella pagina master aggiunta in precedenza si usa un `table` HTML per il layout, un elemento `img` per il logo della società, il testo statico e i controlli server per gestire l'appartenenza comune per il sito. È possibile usare qualsiasi elemento HTML e qualsiasi elemento ASP.NET come parte della pagina master.

Oltre al testo statico e ai controlli che verranno visualizzati in tutte le pagine, la pagina master includerà anche uno o più controlli **ContentPlaceHolder** . Questi controlli segnaposto definiscono le aree in cui verranno visualizzati i contenuti sostituibili. A sua volta, il contenuto sostituibile viene definito nelle pagine di contenuto, ad esempio *default. aspx*, usando il controllo del server di **contenuti** .

#### <a name="adding-image-files"></a>Aggiunta di file di immagine

L'immagine del logo a cui si fa riferimento, insieme a tutte le immagini del prodotto, deve essere aggiunta all'applicazione Web in modo che possano essere visualizzate quando il progetto viene visualizzato in un browser.

#### <a name="download-from-msdn-samples-site"></a>Scaricare dal sito degli esempi MSDN:

[Introduzione con Web form ASP.NET 4,5 Visual Studio 2013 e Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

Il download include le risorse nella cartella *WingtipToys-assets* che vengono usate per creare l'applicazione di esempio.

1. Se non è già stato fatto, scaricare i file di esempio compressi usando il collegamento riportato sopra dal sito degli esempi MSDN.
2. Al termine del download, aprire il file con estensione zip e copiare il contenuto in una cartella locale nel computer.
3. Trovare e aprire la cartella *WingtipToys-assets* .
4. Trascinando e rilasciando, copiare la cartella del *Catalogo* dalla cartella locale alla radice del progetto di applicazione Web nel **Esplora soluzioni** di Visual Studio. 

    ![Interfaccia utente e navigazione-copia file](ui_and_navigation/_static/image1.png)
5. Successivamente, creare una nuova cartella denominata *images* facendo clic con il pulsante destro del mouse sul progetto **WingtipToys** in **Esplora soluzioni** e selezionando **Aggiungi** -&gt; **nuova cartella**.
6. Copiare il file *logo. jpg* dalla cartella *WingtipToys-assets* in **Esplora file** alla cartella *immagini* del progetto di applicazione Web in **Esplora soluzioni** di Visual Studio.
7. Fare clic sull'opzione **Mostra tutti i file** nella parte superiore di **Esplora soluzioni** per aggiornare l'elenco dei file se i nuovi file non sono visibili.  
  
    **Esplora soluzioni** ora Visualizza i file di progetto aggiornati. 

    ![Interfaccia utente e navigazione-Esplora soluzioni](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Aggiunta di pagine

Prima di aggiungere l'esplorazione all'applicazione Web, è necessario innanzitutto aggiungere due nuove pagine a cui si accederà. Più avanti in questa serie di esercitazioni, verranno visualizzati i prodotti e i dettagli del prodotto nelle nuove pagine.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse su **WingtipToys**, scegliere **Aggiungi**e quindi fare clic su **nuovo elemento**.   
 Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **gruppo C# Visual** -&gt; modelli **Web** a sinistra. Selezionare quindi **Web Form con la pagina master** dall'elenco centrale e denominarlo *Production. aspx*. 

    ![Interfaccia utente e navigazione-finestra di dialogo Aggiungi nuovo elemento](ui_and_navigation/_static/image3.png)
3. Selezionare **site. master** per alleghi la pagina master alla pagina *. aspx* appena creata. 

    ![Interfaccia utente e navigazione-selezione della pagina master](ui_and_navigation/_static/image4.png)
4. Aggiungere una pagina aggiuntiva denominata *productDetails. aspx* attenendosi alla stessa procedura.

### <a name="updating-bootstrap"></a>Aggiornamento bootstrap

I modelli di progetto Visual Studio 2013 usano [bootstrap](http://getbootstrap.com/), un layout e un Framework di tema creati da Twitter. Bootstrap USA CSS3 per fornire una progettazione reattiva, il che significa che i layout possono adattarsi dinamicamente alle diverse dimensioni della finestra del browser. È anche possibile usare la funzionalità di tema di bootstrap per applicare facilmente una modifica nell'aspetto dell'applicazione. Per impostazione predefinita, il modello di applicazione Web ASP.NET in Visual Studio 2013 include bootstrap come pacchetto NuGet.

In questa esercitazione verrà modificato l'aspetto dell'applicazione Wingtip Toys sostituendo i file CSS bootstrap.

1. In **Esplora soluzioni**aprire la cartella *contenuto* .
2. Fare clic con il pulsante destro del mouse sul file *bootstrap. CSS* e rinominarlo in *bootstrap-Original. CSS*.
3. Rinominare *bootstrap. min. CSS* in *bootstrap-Original. min. CSS*.
4. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *contenuto* e scegliere **Apri cartella in Esplora file**.  
   Verrà visualizzato Esplora file. Si salveranno i file CSS bootstrap scaricati in questo percorso.
5. Nel browser passare a [https://bootswatch.com/3/](https://bootswatch.com/3/).
6. Scorrere la finestra del browser finché non viene visualizzato il tema. 

    ![Interfaccia utente e navigazione-tema bidimensionale](ui_and_navigation/_static/image5.png)
7. Scaricare sia il file *bootstrap. CSS* sia il file *bootstrap. min. CSS* nella cartella *contenuto* . Usare il percorso della cartella contenuto visualizzata nella finestra **Esplora file** che è stata aperta in precedenza.
8. In **Visual Studio** nella parte superiore di **Esplora soluzioni**selezionare l'opzione **Mostra tutti i file** per visualizzare i nuovi file nella cartella contenuto. 

    ![Interfaccia utente e navigazione-Esplora soluzioni](ui_and_navigation/_static/image6.png)

   I due nuovi file CSS vengono visualizzati nella cartella **Content** , ma si noti che l'icona accanto a ogni nome di file è disabilitata. Questo significa che il file non è ancora stato aggiunto al progetto.
9. Fare clic con il pulsante destro del mouse sui file *bootstrap. CSS* e *bootstrap. min. CSS* e scegliere **Includi nel progetto**.   
   Quando si esegue l'applicazione Wingtip Toys più avanti in questa esercitazione, verrà visualizzata la nuova interfaccia utente.

> [!NOTE] 
> 
> Il modello di applicazione Web ASP.NET usa il file *bundle. config* nella radice del progetto per archiviare il percorso dei file CSS bootstrap.

### <a name="modifying-the-default-navigation"></a>Modifica della navigazione predefinita

La navigazione predefinita per ogni pagina nell'applicazione può essere modificata cambiando l'elemento dell'elenco di navigazione non ordinato presente nella pagina *site. master* .

1. In **Esplora soluzioni**individuare e aprire la pagina *site. master* .
2. Aggiungere il collegamento di navigazione aggiuntivo evidenziato in giallo all'elenco non ordinato mostrato di seguito:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Come si può notare nel codice HTML riportato sopra, è stato modificato ogni elemento riga `<li>` contenente un tag di ancoraggio `<a>` con un attributo di `href` collegamento. Ogni `href` punta a una pagina nell'applicazione Web. Nel browser, quando un utente fa clic su uno di questi collegamenti (ad esempio, **Products**), accederà alla pagina contenuta nella `href` (ad esempio, **Production. aspx**). L'applicazione verrà eseguita alla fine di questa esercitazione.

> [!NOTE] 
> 
> Il carattere tilde (`~`) viene usato per specificare che il percorso di `href` inizia alla radice del progetto.

### <a name="adding-a-data-control-to-display-navigation-data"></a>Aggiunta di un controllo dati per la visualizzazione dei dati di navigazione

Successivamente, si aggiungerà un controllo per visualizzare tutte le categorie dal database. Ogni categoria fungerà da collegamento alla pagina *Production. aspx* . Quando un utente fa clic sul collegamento di una categoria nel browser, accede alla pagina prodotti e visualizza solo i prodotti associati alla categoria selezionata.

Verrà utilizzato un controllo **ListView** per visualizzare tutte le categorie contenute nel database. Per aggiungere un controllo **ListView** alla pagina master:

1. Nella pagina *site. master* aggiungere il seguente elemento `<div>` evidenziato **dopo** l'elemento `<div>` contenente il `id="TitleContent"` aggiunto in precedenza:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Questo codice visualizzerà tutte le categorie del database. Il controllo **ListView** Visualizza il nome di ogni categoria come testo del collegamento e include un collegamento alla pagina *Production. aspx* con un valore della stringa di query contenente il `ID` della categoria. Impostando la proprietà `ItemType` nel controllo **ListView** , l'espressione di associazione dati `Item` è disponibile all'interno del nodo `ItemTemplate` e il controllo diventa fortemente tipizzato. È possibile selezionare i dettagli dell'oggetto `Item` usando IntelliSense, ad esempio specificando il `CategoryName`. Questo codice è contenuto all'interno del contenitore `<%#: %>` che contrassegna un'espressione di associazione dati. Aggiungendo (:) alla fine del prefisso `<%#`, il risultato dell'espressione di associazione dati è codificato in HTML. Quando il risultato è codificato in formato HTML, l'applicazione è più protetta da attacchi di script injection (XSS) e attacchi HTML injection.

> [!NOTE] 
> 
> **Punta**
> 
> Quando si aggiunge codice digitando durante lo sviluppo, è possibile essere certi che un membro valido di un oggetto venga trovato perché i controlli dati fortemente tipizzati mostrano i membri disponibili in base a IntelliSense. IntelliSense offre scelte di codice appropriate per il contesto durante la digitazione di codice, ad esempio proprietà, metodi e oggetti.

Nel passaggio successivo verrà implementato il metodo `GetCategories` per recuperare i dati.

### <a name="linking-the-data-control-to-the-database"></a>Collegamento del controllo dati al database

Prima di poter visualizzare i dati nel controllo dati, è necessario collegare il controllo dati al database. Per creare il collegamento, è possibile modificare il code-behind del file *site.master.cs* .

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla pagina *site. master* , quindi scegliere **Visualizza codice**. Il file *site.master.cs* viene aperto nell'editor.
2. In prossimità dell'inizio del file *site.master.cs* aggiungere due spazi dei nomi aggiuntivi in modo che tutti gli spazi dei nomi inclusi siano visualizzati come segue:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Aggiungere il metodo di `GetCategories` evidenziato dopo il gestore dell'evento `Page_Load`, come indicato di seguito:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Il codice precedente viene eseguito quando una pagina che utilizza la pagina master viene caricata nel browser. Il controllo `ListView` (denominato "CategoryName") aggiunto in precedenza in questa esercitazione usa l'associazione di modelli per selezionare i dati. Nel markup del controllo `ListView` si imposta la proprietà `SelectMethod` del controllo sul metodo `GetCategories`, illustrato in precedenza. Il controllo `ListView` chiama il metodo `GetCategories` al momento appropriato nel ciclo di vita della pagina e associa automaticamente i dati restituiti. Per ulteriori informazioni sull'associazione dei dati, vedere l'esercitazione successiva.

### <a name="running-the-application-and-creating-the-database"></a>Esecuzione dell'applicazione e creazione del database

In precedenza in questa serie di esercitazioni è stata creata una classe inizializzatore (denominata "ProductDatabaseInitializer") e specificata questa classe nel file *Global.asax.cs* . Il Entity Framework genererà il database quando l'applicazione viene eseguita la prima volta perché il metodo `Application_Start` contenuto nel file *Global.asax.cs* chiamerà la classe inizializzatore. La classe inizializzatore utilizzerà le classi del modello (`Category` e `Product`) aggiunte in precedenza in questa serie di esercitazioni per creare il database.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla pagina *default. aspx* e selezionare **Imposta come pagina iniziale**.
2. In Visual Studio premere **F5**.   
 La configurazione di tutti gli elementi durante la prima esecuzione può richiedere molto tempo.   
    ![interfaccia utente e navigazione-](ui_and_navigation/_static/image7.png) Windows browser  
 Quando si esegue l'applicazione, l'applicazione verrà compilata e verrà creato il database denominato *wingtiptoys. MDF* nella cartella *app\_data* . Nel browser viene visualizzato un menu di navigazione delle categorie. Questo menu è stato generato recuperando le categorie dal database. Nell'esercitazione successiva verrà implementata la navigazione.
3. Chiudere il browser per arrestare l'applicazione in esecuzione.

### <a name="reviewing-the-database"></a>Revisione del database

Aprire il file *Web. config* ed esaminare la sezione della stringa di connessione. È possibile osservare che il valore `AttachDbFilename` nella stringa di connessione punta alla `DataDirectory` per il progetto di applicazione Web. Il valore `|DataDirectory|` è un valore riservato che rappresenta la cartella *App\_data* nel progetto. Questa cartella è la posizione in cui si trova il database creato dalle classi di entità.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Se la cartella *App\_data* non è visibile o la cartella è vuota, selezionare l'icona di **aggiornamento** e quindi l'icona **Mostra tutti i file** nella parte superiore della finestra di **Esplora soluzioni** . Potrebbe essere necessario espandere la larghezza delle finestre **Esplora soluzioni** per mostrare tutte le icone disponibili.

A questo punto è possibile esaminare i dati contenuti nel file di database *wingtiptoys. MDF* usando la finestra **Esplora server** .

1. Espandere la cartella *App\_data* . Se la cartella *App\_data* non è visibile, vedere la nota sopra.
2. Se il file di database *wingtiptoys. MDF* non è visibile, selezionare l'icona di **aggiornamento** e quindi l'icona **Mostra tutti i file** nella parte superiore della finestra di **Esplora soluzioni** .
3. Fare clic con il pulsante destro del mouse sul file di database *wingtiptoys. MDF* e scegliere **Apri**.  
    Viene visualizzato **Esplora server** . 

    ![Interfaccia utente e navigazione-Esplora server](ui_and_navigation/_static/image8.png)
4. Espandere la cartella *tabelle* .
5. Fare clic con il pulsante destro del mouse sulla tabella **Products**e scegliere **Mostra dati tabella**.  
 Viene visualizzata la tabella **Products** . 

    ![Interfaccia utente e navigazione-tabella Products](ui_and_navigation/_static/image9.png)
6. Questa visualizzazione consente di visualizzare e modificare manualmente i dati nella tabella **Products** .
7. Chiudere la finestra tabella **Products** .
8. Nella **Esplora server**, fare di nuovo clic con il pulsante destro del mouse sulla tabella **Products** e selezionare **Apri definizione tabella**.  
 Viene visualizzata la progettazione dei dati per la tabella **Products** . 

    ![Interfaccia utente e navigazione-progettazione prodotti](ui_and_navigation/_static/image10.png)
9. Nella scheda **T-SQL** viene visualizzata l'istruzione SQL DDL utilizzata per creare la tabella. È inoltre possibile utilizzare l'interfaccia utente nella scheda **progettazione** per modificare lo schema.
10. Nella **Esplora server**fare clic con il pulsante destro del mouse su **WingtipToys** database e scegliere **Chiudi connessione**.   
 Scollegando il database da Visual Studio, lo schema del database sarà in grado di essere modificato più avanti in questa serie di esercitazioni.
11. Tornare a **Esplora soluzioni**selezionando la scheda **Esplora soluzioni** nella parte inferiore della finestra **Esplora server** .

## <a name="summary"></a>Riepilogo

In questa esercitazione della serie sono state aggiunte alcune interfacce utente, elementi grafici, pagine e navigazione di base. Inoltre, è stata eseguita l'applicazione Web, che ha creato il database dalle classi di dati aggiunte nell'esercitazione precedente. È stato inoltre visualizzato il contenuto della tabella *Products* del database visualizzando direttamente il database. Nell'esercitazione successiva verranno visualizzati gli elementi di dati e i dettagli del database.

## <a name="additional-resources"></a>Risorse aggiuntive

[Introduzione alla programmazione Pagine Web ASP.NET](https://msdn.microsoft.com/library/ms178125.aspx)   
[Cenni preliminari sui controlli server Web di ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Esercitazione CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Precedente](create_the_data_access_layer.md)
> [Successivo](display_data_items_and_details.md)
