---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Gestione degli errori ASP.NET | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 9514142ca50b33470a3f4c033e4f8e319a9ee09b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566682"
---
# <a name="aspnet-error-handling"></a>Gestione degli errori di ASP.NET

di [Erik Reitan](https://github.com/Erikre)

[Scaricare il progetto di esempio WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare l'E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013 per il Web. Per accompagnare questa serie di esercitazioni è disponibile un [progetto Visual Studio 2013 C# con codice sorgente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) .

In questa esercitazione verrà modificata l'applicazione di esempio Wingtip Toys per includere la gestione degli errori e la registrazione degli errori. La gestione degli errori consentirà all'applicazione di gestire correttamente gli errori e di visualizzare i messaggi di errore di conseguenza. La registrazione degli errori consentirà di individuare e correggere gli errori che si sono verificati. Questa esercitazione si basa sull'esercitazione precedente "routing degli URL" ed è parte della serie di esercitazioni su Wingtip Toys.

## <a name="what-youll-learn"></a>Contenuto dell'esercitazione:

- Come aggiungere la gestione degli errori globale alla configurazione dell'applicazione.
- Come aggiungere la gestione degli errori a livello di applicazione, pagina e codice.
- Come registrare gli errori per una verifica successiva.
- Come visualizzare i messaggi di errore che non compromettono la sicurezza.
- Come implementare la registrazione degli errori di registrazione e di gestione errori (ELMAH).

## <a name="overview"></a>Panoramica

Le applicazioni ASP.NET devono essere in grado di gestire in modo coerente gli errori che si verificano durante l'esecuzione. ASP.NET usa il Common Language Runtime (CLR), che fornisce un modo per notificare le applicazioni degli errori in modo uniforme. Quando si verifica un errore, viene generata un'eccezione. Un'eccezione è rappresentata da qualsiasi errore, condizione o comportamento imprevisto rilevato da un'applicazione.

In .NET Framework, un'eccezione è un oggetto che eredita dalla classe `System.Exception`. Le eccezioni vengono generate dalle aree di codice in cui si è verificato un problema. L'eccezione viene passata allo stack di chiamate in una posizione in cui l'applicazione fornisce il codice per gestire l'eccezione. Se l'applicazione non gestisce l'eccezione, il browser verrà forzato a visualizzare i dettagli dell'errore.

Come procedura consigliata, gestire gli errori in a livello di codice in `Try`/`Catch`/blocchi `Finally` all'interno del codice. Provare a collocare questi blocchi in modo che l'utente possa correggere i problemi nel contesto in cui si verificano. Se i blocchi di gestione degli errori sono troppo lontani da dove si è verificato l'errore, diventa più difficile fornire agli utenti le informazioni necessarie per risolvere il problema.

### <a name="exception-class"></a>Classe Exception

La classe Exception è la classe di base da cui ereditano le eccezioni. La maggior parte degli oggetti eccezione sono istanze di una classe derivata della classe Exception, ad esempio la classe `SystemException`, la classe `IndexOutOfRangeException` o la classe `ArgumentNullException`. La classe Exception dispone di proprietà, ad esempio la proprietà `StackTrace`, la proprietà `InnerException` e la proprietà `Message`, che forniscono informazioni specifiche sull'errore che si è verificato.

### <a name="exception-inheritance-hierarchy"></a>Gerarchia di ereditarietà delle eccezioni

Il runtime dispone di un set di base di eccezioni derivanti dalla classe `SystemException` generata dal runtime quando viene rilevata un'eccezione. La maggior parte delle classi che ereditano dalla classe Exception, ad esempio la classe `IndexOutOfRangeException` e la classe `ArgumentNullException`, non implementano membri aggiuntivi. Pertanto, le informazioni più importanti per un'eccezione si trovano nella gerarchia delle eccezioni, nel nome dell'eccezione e nelle informazioni contenute nell'eccezione.

### <a name="exception-handling-hierarchy"></a>Gerarchia di gestione delle eccezioni

In un'applicazione Web Form ASP.NET, le eccezioni possono essere gestite in base a una gerarchia di gestione specifica. Un'eccezione può essere gestita ai livelli seguenti:

- Livello applicazione
- A livello di pagina
- Livello di codice

Quando un'applicazione gestisce le eccezioni, è spesso possibile recuperare e visualizzare all'utente informazioni aggiuntive sull'eccezione ereditata dalla classe di eccezione. Oltre al livello di applicazione, pagina e codice, è anche possibile gestire le eccezioni a livello di modulo HTTP e usando un gestore personalizzato di IIS.

### <a name="application-level-error-handling"></a>Gestione degli errori a livello di applicazione

È possibile gestire gli errori predefiniti a livello di applicazione modificando la configurazione dell'applicazione o aggiungendo un gestore `Application_Error` nel file *Global. asax* dell'applicazione.

È possibile gestire gli errori predefiniti e gli errori HTTP aggiungendo una sezione `customErrors` al file *Web. config* . La sezione `customErrors` consente di specificare una pagina predefinita a cui gli utenti verranno reindirizzati quando si verifica un errore. Consente inoltre di specificare singole pagine per errori specifici del codice di stato.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Sfortunatamente, quando si usa la configurazione per reindirizzare l'utente a una pagina diversa, non sono disponibili i dettagli dell'errore che si è verificato.

È tuttavia possibile intercettare gli errori che si verificano in qualsiasi punto dell'applicazione aggiungendo il codice al gestore `Application_Error` nel file *Global. asax* .

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Gestione degli eventi di errore a livello di pagina

Un gestore a livello di pagina restituisce all'utente la pagina in cui si è verificato l'errore, ma poiché le istanze dei controlli non vengono mantenute, non sarà più possibile eseguire alcuna operazione nella pagina. Per fornire i dettagli dell'errore all'utente dell'applicazione, è necessario scrivere in modo specifico i dettagli dell'errore nella pagina.

Si usa in genere un gestore degli errori a livello di pagina per registrare gli errori non gestiti o per portare l'utente a una pagina in grado di visualizzare informazioni utili.

Questo esempio di codice mostra un gestore per l'evento di errore in una pagina Web ASP.NET. Questo gestore intercetta tutte le eccezioni che non sono già gestite entro `try`/blocchi `catch` nella pagina.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Dopo aver gestito un errore, è necessario cancellarlo chiamando il metodo di `ClearError` dell'oggetto server (classe`HttpServerUtility`). in caso contrario, verrà visualizzato un errore che si è verificato in precedenza.

### <a name="code-level-error-handling"></a>Gestione degli errori a livello di codice

L'istruzione try-catch è costituita da un blocco try seguito da una o più clausole catch, che specificano i gestori per eccezioni diverse. Quando viene generata un'eccezione, il Common Language Runtime (CLR) Cerca l'istruzione catch che gestisce questa eccezione. Se il metodo attualmente in esecuzione non contiene un blocco catch, CLR analizza il metodo che ha chiamato il metodo corrente e così via lo stack di chiamate. Se non viene trovato alcun blocco catch, CLR Visualizza un messaggio di eccezione non gestita per l'utente e interrompe l'esecuzione del programma.

Nell'esempio di codice riportato di seguito viene illustrato un modo comune per utilizzare `try`/`catch`/`finally` per gestire gli errori.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Nel codice precedente il blocco try contiene il codice che deve essere sorvegliato da una possibile eccezione. Il blocco viene eseguito fino a quando non viene generata un'eccezione o il blocco viene completato correttamente. Se si verifica un'eccezione `FileNotFoundException` o un'eccezione `IOException`, l'esecuzione viene trasferita a una pagina diversa. Il codice contenuto nel blocco finally viene quindi eseguito, indipendentemente dal fatto che si sia verificato o meno un errore.

## <a name="adding-error-logging-support"></a>Aggiunta del supporto per la registrazione degli errori

Prima di aggiungere la gestione degli errori all'applicazione di esempio Wingtip Toys, verrà aggiunto il supporto per la registrazione degli errori aggiungendo una classe `ExceptionUtility` alla cartella *logica* . In questo modo, ogni volta che l'applicazione gestisce un errore, i dettagli dell'errore verranno aggiunti al file di log degli errori.

1. Fare clic con il pulsante destro del mouse sulla cartella *logica* , quindi scegliere **Aggiungi** -&gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **gruppo C# Visual** -&gt; modelli di **codice** a sinistra. Quindi, selezionare **classe**nell'elenco centrale e denominarla **ExceptionUtility.cs**.
3. Scegliere **Aggiungi**. Viene visualizzato il nuovo file di classe.
4. Sostituire il codice esistente con quello seguente:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Quando si verifica un'eccezione, l'eccezione può essere scritta in un file di log delle eccezioni chiamando il metodo `LogException`. Questo metodo accetta due parametri, l'oggetto eccezione e una stringa contenente i dettagli sull'origine dell'eccezione. Il log delle eccezioni viene scritto nel file *log degli errori* nella cartella *app\_data* .

### <a name="adding-an-error-page"></a>Aggiunta di una pagina di errore

Nell'applicazione di esempio Wingtip Toys verrà usata una pagina per visualizzare gli errori. La pagina di errore è progettata per mostrare un messaggio di errore protetto agli utenti del sito. Tuttavia, se l'utente è uno sviluppatore che effettua una richiesta HTTP che viene servita localmente nel computer in cui si trova il codice, nella pagina di errore verranno visualizzati ulteriori dettagli sull'errore.

1. Fare clic con il pulsante destro del mouse sul nome del progetto (**Wingtip Toys**) in **Esplora soluzioni** e scegliere **Aggiungi** -&gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **gruppo C# Visual** -&gt; modelli **Web** a sinistra. Dall'elenco centrale selezionare **Web Form con la pagina master**e denominarlo **ErrorPage. aspx**.
3. Fare clic su **Aggiungi**.
4. Selezionare il file *site. master* come pagina master, quindi scegliere **OK**.
5. Sostituire il markup esistente con il codice seguente:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Sostituire il codice esistente del code-behind (*ErrorPage.aspx.cs*) in modo che venga visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Quando viene visualizzata la pagina di errore, viene eseguito il gestore dell'evento `Page_Load`. Nel gestore `Page_Load` viene determinata la posizione in cui è stato gestito il primo errore. Quindi, l'ultimo errore che si è verificato è determinato dalla chiamata al metodo `GetLastError` dell'oggetto server. Se l'eccezione non esiste più, viene creata un'eccezione generica. Quindi, se la richiesta HTTP è stata effettuata localmente, vengono visualizzati tutti i dettagli dell'errore. In questo caso, solo il computer locale che esegue l'applicazione Web visualizzerà i dettagli dell'errore. Una volta visualizzate le informazioni sull'errore, l'errore viene aggiunto al file di log e l'errore viene cancellato dal server.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Visualizzazione di messaggi di errore non gestiti per l'applicazione

Aggiungendo una sezione `customErrors` al file *Web. config* , è possibile gestire rapidamente gli errori semplici che si verificano in tutta l'applicazione. È anche possibile specificare come gestire gli errori in base al relativo valore del codice di stato, ad esempio 404-file non trovato.

#### <a name="update-the-configuration"></a>Aggiornare la configurazione

Aggiornare la configurazione aggiungendo una sezione `customErrors` al file *Web. config* .

1. In **Esplora soluzioni**individuare e aprire il file *Web. config* nella radice dell'applicazione di esempio Wingtip Toys.
2. Aggiungere la sezione `customErrors` al file *Web. config* all'interno del nodo `<system.web>`, come indicato di seguito:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Salvare il file *Web.config* .

La sezione `customErrors` specifica la modalità, impostata su "on". Specifica inoltre la `defaultRedirect`, che indica all'applicazione la pagina a cui passare quando si verifica un errore. È stato inoltre aggiunto un elemento Error specifico che specifica come gestire un errore 404 quando non viene trovata una pagina. Più avanti in questa esercitazione verrà aggiunta la gestione degli errori aggiuntiva che acquisirà i dettagli di un errore a livello di applicazione.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire ora l'applicazione per visualizzare le route aggiornate.

1. Premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 Verrà aperto il browser e verrà visualizzata la pagina *default. aspx* .
2. Immettere l'URL seguente nel browser (assicurarsi **di usare il** numero di porta):  
    `https://localhost:44300/NoPage.aspx`
3. Esaminare *ErrorPage. aspx* visualizzato nel browser. 

    ![Gestione degli errori di ASP.NET-errore di pagina non trovata](aspnet-error-handling/_static/image1.png)

Quando si richiede la pagina *NOPAGE. aspx* , che non esiste, nella pagina di errore viene visualizzato il messaggio di errore semplice e le informazioni dettagliate sull'errore se sono disponibili ulteriori dettagli. Tuttavia, se l'utente ha richiesto una pagina non esistente da una posizione remota, nella pagina di errore viene visualizzato solo il messaggio di errore in rosso.

### <a name="including-an-exception-for-testing-purposes"></a>Inclusione di un'eccezione a scopo di test

Per verificare il funzionamento dell'applicazione quando si verifica un errore, è possibile creare intenzionalmente condizioni di errore in ASP.NET. Nell'applicazione di esempio Wingtip Toys viene generata un'eccezione di test quando viene caricata la pagina predefinita per vedere cosa accade.

1. Aprire il code-behind della pagina *default. aspx* in Visual Studio.   
   Verrà visualizzata la pagina code-behind *default.aspx.cs* .
2. Nel gestore `Page_Load` aggiungere il codice in modo che il gestore appaia come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

È possibile creare diversi tipi di eccezioni. Nel codice precedente si sta creando una `InvalidOperationException` quando viene caricata la pagina *default. aspx* .

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione per verificare il modo in cui l'applicazione gestisce l'eccezione.

1. Premere **CTRL + F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 L'applicazione genera l'eccezione InvalidOperationException. 

    > [!NOTE] 
    > 
    > È necessario premere **CTRL + F5** per visualizzare la pagina senza suddividere il codice per visualizzare l'origine dell'errore in Visual Studio.
2. Esaminare *ErrorPage. aspx* visualizzato nel browser. 

    ![Gestione degli errori di ASP.NET-pagina errore](aspnet-error-handling/_static/image2.png)

Come si può notare nei dettagli dell'errore, l'eccezione è stata intercettata dalla sezione `customError` nel file *Web. config* .

### <a name="adding-application-level-error-handling"></a>Aggiunta della gestione degli errori a livello di applicazione

Anziché intercettare l'eccezione utilizzando la sezione `customErrors` nel file *Web. config* , in cui si ottengono poche informazioni sull'eccezione, è possibile intercettare l'errore a livello di applicazione e recuperare i dettagli dell'errore.

1. In **Esplora soluzioni**individuare e aprire il file *Global.asax.cs* .
2. Aggiungere un gestore degli **errori di\_dell'applicazione** in modo che venga visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Quando si verifica un errore nell'applicazione, viene chiamato il gestore `Application_Error`. In questo gestore, l'ultima eccezione viene recuperata e esaminata. Se l'eccezione non è stata gestita e l'eccezione contiene dettagli dell'eccezione interna (ovvero `InnerException` non è null), l'applicazione trasferisce l'esecuzione alla pagina di errore in cui vengono visualizzati i dettagli dell'eccezione.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire l'applicazione per visualizzare i dettagli dell'errore aggiuntivi forniti gestendo l'eccezione a livello di applicazione.

1. Premere **CTRL + F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 L'applicazione genera l'`InvalidOperationException`.
2. Esaminare *ErrorPage. aspx* visualizzato nel browser. 

    ![Gestione degli errori di ASP.NET-errore a livello di applicazione](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Aggiunta della gestione degli errori a livello di pagina

È possibile aggiungere la gestione degli errori a livello di pagina a una pagina usando l'aggiunta di un attributo `ErrorPage` alla direttiva `@Page` della pagina o aggiungendo un gestore eventi `Page_Error` al code-behind di una pagina. In questa sezione verrà aggiunto un gestore dell'evento `Page_Error` che trasferirà l'esecuzione alla pagina *ErrorPage. aspx* .

1. In **Esplora soluzioni**individuare e aprire il file *default.aspx.cs* .
2. Aggiungere un gestore `Page_Error` in modo che il code-behind venga visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Quando si verifica un errore nella pagina, viene chiamato il gestore dell'evento `Page_Error`. In questo gestore, l'ultima eccezione viene recuperata e esaminata. Se si verifica un `InvalidOperationException`, il gestore dell'evento `Page_Error` trasferisce l'esecuzione alla pagina di errore in cui vengono visualizzati i dettagli dell'eccezione.

#### <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire ora l'applicazione per visualizzare le route aggiornate.

1. Premere **CTRL + F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 L'applicazione genera l'`InvalidOperationException`.
2. Esaminare *ErrorPage. aspx* visualizzato nel browser. 

    ![Gestione degli errori di ASP.NET-errore a livello di pagina](aspnet-error-handling/_static/image4.png)
3. Chiudere la finestra del browser.

### <a name="removing-the-exception-used-for-testing"></a>Rimozione dell'eccezione utilizzata per il test

Per consentire il funzionamento dell'applicazione di esempio Wingtip Toys senza generare l'eccezione aggiunta in precedenza in questa esercitazione, rimuovere l'eccezione.

1. Aprire il code-behind della pagina *default. aspx* .
2. Nel gestore `Page_Load` rimuovere il codice che genera l'eccezione in modo che il gestore appaia come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Aggiunta della registrazione degli errori a livello di codice

Come indicato in precedenza in questa esercitazione, è possibile aggiungere istruzioni try/catch per tentare di eseguire una sezione di codice e gestire il primo errore che si verifica. In questo esempio, i dettagli dell'errore verranno scritti nel file di log degli errori in modo che l'errore possa essere esaminato in un secondo momento.

1. In **Esplora soluzioni**, nella cartella *logica* , trovare e aprire il file *PayPalFunctions.cs* .
2. Aggiornare il metodo `HttpCall` in modo che il codice venga visualizzato come segue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Il codice precedente chiama il metodo `LogException` contenuto nella classe `ExceptionUtility`. Il file della classe *ExceptionUtility.cs* è stato aggiunto alla cartella *logica* in precedenza in questa esercitazione. Il metodo `LogException` accetta due parametri. Il primo parametro è l'oggetto Exception. Il secondo parametro è una stringa utilizzata per identificare l'origine dell'errore.

### <a name="inspecting-the-error-logging-information"></a>Esaminare le informazioni di registrazione degli errori

Come indicato in precedenza, è possibile usare il log degli errori per determinare quali errori nell'applicazione devono essere corretti per primi. Naturalmente, verranno registrati solo gli errori che sono stati intercettati e scritti nel log degli errori di.

1. In **Esplora soluzioni**individuare e aprire il file *log degli errori. txt* nella cartella *app\_data* .   
 Potrebbe essere necessario selezionare l'opzione "**Mostra tutti i file**" o "**Aggiorna**" nella parte superiore di **Esplora soluzioni** per visualizzare il file *log degli errori. txt* .
2. Esaminare il log degli errori visualizzato in Visual Studio: 

    ![Gestione degli errori di ASP.NET-log degli errori](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Messaggi di errore sicuri

È **importante tenere presente** che, quando l'applicazione Visualizza messaggi di errore, non deve fornire informazioni che un utente malintenzionato potrebbe trovare utile per attaccare l'applicazione. Se, ad esempio, l'applicazione tenta di scrivere in un database in modo non corretto, non dovrebbe visualizzare un messaggio di errore che include il nome utente utilizzato. Per questo motivo, all'utente viene visualizzato un messaggio di errore generico in rosso. Tutti i dettagli aggiuntivi sull'errore vengono visualizzati solo per lo sviluppatore nel computer locale.

## <a name="using-elmah"></a>Uso di ELMAH

ELMAH (Error Logging Modules and Handlers) è una funzionalità di registrazione degli errori che si collega all'applicazione ASP.NET come pacchetto NuGet. ELMAH offre le funzionalità seguenti:

- Registrazione di eccezioni non gestite.
- Pagina Web per visualizzare l'intero log delle eccezioni non gestite ricodificate.
- Una pagina Web per visualizzare i dettagli completi di ogni eccezione registrata.
- Una notifica di posta elettronica di ogni errore nel momento in cui si verifica.
- Un feed RSS degli ultimi 15 errori dal log.

Prima di poter usare ELMAH, è necessario installarlo. Questa operazione è semplice usando il programma di installazione del pacchetto *NuGet* . Come indicato in precedenza in questa serie di esercitazioni, NuGet è un'estensione di Visual Studio che semplifica l'installazione e l'aggiornamento di librerie e strumenti open source in Visual Studio.

1. In Visual Studio scegliere **Gestione pacchetti NuGet** dal menu **strumenti** > **Gestisci pacchetti NuGet per la soluzione**. 

    ![Gestione degli errori di ASP.NET-Gestisci pacchetti NuGet per la soluzione](aspnet-error-handling/_static/image6.png)
2. La finestra di dialogo **Gestisci pacchetti NuGet** viene visualizzata in Visual Studio.
3. Nella finestra di dialogo **Gestisci pacchetti NuGet** espandere **online** a sinistra e quindi selezionare **NuGet.org**. Individuare e installare il pacchetto **ELMAH** dall'elenco dei pacchetti disponibili online. 

    ![Gestione degli errori di ASP.NET-pacchetto NuGet di ELMA](aspnet-error-handling/_static/image7.png)
4. Per scaricare il pacchetto, è necessario disporre di una connessione Internet.
5. Nella finestra di dialogo **Seleziona progetti** verificare che sia selezionata la selezione **WingtipToys** , quindi fare clic su **OK**. 

    ![Gestione degli errori di ASP.NET-finestra di dialogo Seleziona progetti](aspnet-error-handling/_static/image8.png)
6. Se necessario, fare clic su **Chiudi** nella finestra di dialogo **Gestisci pacchetti NuGet** .
7. Se Visual Studio richiede di ricaricare i file aperti, selezionare "**Sì a tutti**".
8. Il pacchetto ELMAH aggiunge le voci per se stesso nel file *Web. config* nella radice del progetto. Se Visual Studio chiede se si vuole ricaricare il file *Web. config* modificato, fare clic su **Sì**.

ELMAH è ora pronto per archiviare gli eventuali errori non gestiti che si verificano.

### <a name="viewing-the-elmah-log"></a>Visualizzazione del log ELMAH

La visualizzazione del log ELMAH è facile, ma prima verrà creata un'eccezione non gestita che verrà registrata nel log di ELMAH.

1. Premere **CTRL + F5** per eseguire l'applicazione di esempio Wingtip Toys.
2. Per scrivere un'eccezione non gestita nel registro ELMAH, navigare nel browser sull'URL seguente (usando il numero di porta):  
    `https://localhost:44300/NoPage.aspx` verrà visualizzata la pagina di errore.
3. Per visualizzare il log di ELMAH, passare all'URL seguente (usando il numero di porta) nel browser:  
    `https://localhost:44300/elmah.axd`

    ![Gestione degli errori di ASP.NET-log degli errori ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Riepilogo

In questa esercitazione si è appreso come gestire gli errori a livello di applicazione, a livello di pagina e a livello di codice. Si è inoltre appreso come registrare gli errori gestiti e non gestiti per una revisione successiva. È stata aggiunta l'utilità ELMAH per fornire la registrazione delle eccezioni e la notifica all'applicazione tramite NuGet. Si è inoltre appreso l'importanza dei messaggi di errore sicuri.

## <a name="tutorial-series-conclusion"></a>Conclusione della serie di esercitazioni

Grazie per aver seguito l'accordo. Spero che questo set di esercitazioni abbia permesso di acquisire familiarità con ASP.NET Web Forms. Per ulteriori informazioni sulle funzionalità di Web Form disponibili in ASP.NET 4,5 e Visual Studio 2013, vedere [ASP.NET and Web Tools per Visual Studio 2013 Note sulla versione](../../../../visual-studio/overview/2013/release-notes.md). Vedere anche l'esercitazione citata nella sezione **passaggi successivi** e defintely provare la [versione di valutazione gratuita di Azure](https://azure.microsoft.com/pricing/free-trial/).

![Grazie-Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla distribuzione dell'applicazione Web in Microsoft Azure, vedere [distribuire un'app web forms ASP.NET sicura con appartenenza, OAuth e database SQL in un sito Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Versione di valutazione gratuita

[Versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)  
 La pubblicazione del sito Web per Microsoft Azure consentirà di risparmiare tempo, manutenzione e spese. Si tratta di un processo rapido per la distribuzione dell'app Web in Azure. Quando è necessario gestire e monitorare l'app Web, Azure offre un'ampia gamma di strumenti e servizi. Gestisci i dati, il traffico, l'identità, i backup, la messaggistica, i supporti e le prestazioni in Azure. In questo modo viene fornito un approccio molto conveniente.

## <a name="additional-resources"></a>Risorse aggiuntive

[Registrazione dei dettagli degli errori con ASP.NET Health monitoring](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Riconoscimenti

Desidero ringraziare i seguenti utenti che hanno apportato contributi significativi al contenuto di questa serie di esercitazioni:

- [Alberto Poblacion, MVP &amp; MCT, Spagna](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Paesi Bassi](http://blog.alexthissen.nl/) (twitter: [@alexthissen](http://twitter.com/alexthissen))
- [Andre Tournier, Stati Uniti](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slovenia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brasile](http://msmvps.com/blogs/bsonnino) (twitter: [@bsonnino](http://twitter.com/bsonnino))
- [Carlos Dos Santos, Brasile](http://www.carloscds.net/)
- [Dave Campbell, USA](http://www.wynapse.com/) (twitter: [@windowsdevnews](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Michael Sharps, USA](http://www.930solutions.com/) (twitter: [@mrsharps](http://twitter.com/mrsharps))
- Mike Pope
- [Venditori, USA](http://www.mitchelsellers.com/) (twitter: [@MitchelSellers](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portogallo](http://paulomorgado.net/)
- [Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Contributi della community

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Esempio di codice correlato di Visual Studio 2012 su MSDN: [navigazione Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Esempio di codice correlato di Visual Studio 2012 su MSDN: [serie di esercitazioni su Web form ASP.NET 4,5 in Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo-collaboratore di pubblico tecnico Microsoft (Twitter: @driazevedo)  
  Visual Studio 2012 Translation: [iniciando com ASP.NET Web forms 4,5-parte 1-Introdução e Visão geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Precedente](url-routing.md)
