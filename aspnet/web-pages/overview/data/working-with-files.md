---
uid: web-pages/overview/data/working-with-files
title: Uso dei file in un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: In questo capitolo viene illustrato come leggere, scrivere, accodare, eliminare e caricare file.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 684c47a8a8480dc040e5144144577c94c35d39e5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642366"
---
# <a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Uso dei file in un sito di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come leggere, scrivere, accodare, eliminare e caricare file in un sito di Pagine Web ASP.NET (Razor).
> 
> > [!NOTE]
> > Se si desidera caricare immagini e modificarle (ad esempio, capovolgerle o ridimensionarle), vedere [utilizzo di immagini in un sito di pagine Web ASP.NET](/aspnet/web-pages/overview/ui-layouts-and-themes/9-working-with-images).
> 
> 
> **Cosa si apprenderà:** 
> 
> - Come creare un file di testo e scrivervi dati.
> - Come accodare i dati a un file esistente.
> - Come leggere un file e visualizzarlo.
> - Come eliminare i file da un sito Web.
> - Come consentire agli utenti di caricare un file o più file.
> 
> Queste sono le funzionalità di programmazione di ASP.NET introdotte nell'articolo:
> 
> - Oggetto `File`, che fornisce un modo per gestire i file.
> - Helper `FileUpload`.
> - Oggetto `Path`, che fornisce i metodi che consentono di modificare il percorso e i nomi file.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Questa esercitazione funziona anche con WebMatrix 3.

<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Creazione di un file di testo e scrittura di dati

Oltre a usare un database nel sito Web, è possibile usare i file. Ad esempio, è possibile utilizzare file di testo come metodo semplice per archiviare i dati per il sito. Un file di testo usato per archiviare i dati viene talvolta denominato *file flat*. I file di testo possono essere in formati diversi, ad esempio con *estensione txt*, *XML*o *CSV* (valori delimitati da virgole).

Se si desidera archiviare i dati in un file di testo, è possibile utilizzare il metodo `File.WriteAllText` per specificare il file da creare e i dati da scrivere. In questa procedura verrà creata una pagina che contiene un semplice form con tre elementi `input` (nome, cognome e indirizzo di posta elettronica) e un pulsante **Submit (Invia** ). Quando l'utente invia il modulo, l'input dell'utente verrà archiviato in un file di testo.

1. Creare una nuova cartella denominata *App\_data*, se non esiste già.
2. Alla radice del sito Web, creare un nuovo file denominato *UserData. cshtml*.
3. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Il markup HTML crea il form con le tre caselle di testo. Nel codice si usa la proprietà `IsPost` per determinare se la pagina è stata inviata prima di iniziare l'elaborazione.

    La prima attività consiste nell'ottenere l'input dell'utente e assegnarlo alle variabili. Il codice concatena quindi i valori delle variabili separate in una stringa delimitata da virgole, che viene quindi archiviata in una variabile diversa. Si noti che il separatore virgola è una stringa racchiusa tra virgolette (",") perché si sta letteralmente incorporando una virgola nella grande stringa che si sta creando. Alla fine dei dati concatenati, aggiungere `Environment.NewLine`. Viene aggiunta un'interruzioni di riga (carattere di nuova riga). Ciò che si sta creando con questa concatenazione è una stringa simile alla seguente:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Con un'interruzioni di riga invisibile alla fine).

    Si crea quindi una variabile (`dataFile`) che contiene il percorso e il nome del file in cui archiviare i dati. Per impostare il percorso è necessaria una gestione speciale. Nei siti Web non è consigliabile fare riferimento al codice in percorsi assoluti come *C:\Folder\File.txt* per i file sul server Web. Se un sito Web viene spostato, il percorso assoluto sarà errato. Inoltre, per un sito ospitato, anziché nel computer in uso, in genere non si conosce il percorso corretto quando si scrive il codice.

    Tuttavia, a volte, ad esempio per la scrittura di un file, è necessario un percorso completo. La soluzione consiste nell'usare il metodo `MapPath` dell'oggetto `Server`. Viene restituito il percorso completo del sito Web. Per ottenere il percorso per la radice del sito Web, è necessario che l'operatore `~` (per riportare la radice virtuale del sito) `MapPath`. È anche possibile passare il nome di una sottocartella, ad esempio *~/App\_data/* , per ottenere il percorso della sottocartella. È quindi possibile concatenare informazioni aggiuntive su qualsiasi valore restituito dal metodo per creare un percorso completo. In questo esempio si aggiunge un nome di file. Per altre informazioni su come usare i percorsi di file e cartelle, vedere [Introduzione alla programmazione pagine Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).

    Il file viene salvato nella cartella *App\_data* . Questa cartella è una cartella speciale in ASP.NET usata per archiviare i file di dati, come descritto in [Introduzione all'uso di un database in siti pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=195209).

    Il `WriteAllText` metodo dell'oggetto `File` scrive i dati nel file. Questo metodo accetta due parametri: il nome (con percorso) del file in cui scrivere e i dati effettivi da scrivere. Si noti che il nome del primo parametro ha un `@` carattere come prefisso. Ciò indica a ASP.NET che si sta fornendo un valore letterale stringa Verbatim e che i caratteri come "/" non devono essere interpretati in modo speciale. Per altre informazioni, vedere [Introduzione alla programmazione Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).

    > [!NOTE]
    > Per consentire al codice di salvare i file nella cartella *App\_data* , l'applicazione deve disporre delle autorizzazioni di lettura/scrittura per la cartella. Nel computer di sviluppo non si tratta in genere di un problema. Tuttavia, quando si pubblica il sito in un server Web del provider di hosting, potrebbe essere necessario impostare in modo esplicito tali autorizzazioni. Se si esegue questo codice nel server di un provider di hosting e si ottengono errori, rivolgersi al provider di hosting per scoprire come impostare tali autorizzazioni.

- Eseguire la pagina in un browser. 

    ![](working-with-files/_static/image1.jpg)
- Immettere i valori nei campi e quindi fare clic su **Invia**.
- Chiudere il browser.
- Tornare al progetto e aggiornare la visualizzazione.
- Aprire il file *Data. txt* . I dati inviati nel modulo si trova nel file. 

    ![[immagine]](working-with-files/_static/image2.jpg)
- Chiudere il file *Data. txt* .

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Aggiunta di dati a un file esistente

Nell'esempio precedente è stato usato `WriteAllText` per creare un file di testo contenente solo una parte di dati. Se si chiama nuovamente il metodo e si passa lo stesso nome di file, il file esistente viene sovrascritto completamente. Tuttavia, dopo aver creato un file, è spesso necessario aggiungere nuovi dati alla fine del file. Questa operazione può essere eseguita usando il metodo `AppendAllText` dell'oggetto `File`.

1. Nel sito Web, creare una copia del file *UserData. cshtml* e denominare la copia *UserDataMultiple. cshtml*.
2. Sostituire il blocco di codice prima del tag `<!DOCTYPE html>` di apertura con il blocco di codice seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Questo codice presenta una modifica rispetto all'esempio precedente. Anziché utilizzare `WriteAllText`, viene utilizzato `the AppendAllText` metodo. I metodi sono simili, ad eccezione del fatto che `AppendAllText` aggiunge i dati alla fine del file. Come con `WriteAllText`, `AppendAllText` crea il file se non esiste già.
3. Eseguire la pagina in un browser.
4. Immettere i valori per i campi e quindi fare clic su **Invia**.
5. Aggiungere altri dati e inviare di nuovo il modulo.
6. Tornare al progetto, fare clic con il pulsante destro del mouse sulla cartella del progetto, quindi scegliere **Aggiorna**.
7. Aprire il file *Data. txt* . Contiene ora i nuovi dati appena immessi. 

    ![[immagine]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Lettura e visualizzazione di dati da un file

Anche se non è necessario scrivere dati in un file di testo, probabilmente a volte è necessario leggere i dati da un file. A tale scopo, è possibile usare nuovamente l'oggetto `File`. È possibile usare l'oggetto `File` per leggere ogni riga singolarmente (separate da interruzioni di riga) o per leggere singoli elementi, indipendentemente dal modo in cui sono separati.

Questa procedura illustra come leggere e visualizzare i dati creati nell'esempio precedente.

1. Alla radice del sito Web, creare un nuovo file denominato *displayData. cshtml*.
2. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Il codice inizia leggendo il file creato nell'esempio precedente in una variabile denominata `userData`, usando questa chiamata al metodo:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Il codice per eseguire questa operazione è all'interno di un'istruzione `if`. Quando si vuole leggere un file, è consigliabile usare il metodo `File.Exists` per determinare prima se il file è disponibile. Il codice controlla anche se il file è vuoto.

    Il corpo della pagina contiene due cicli di `foreach`, uno annidato all'interno dell'altro. Il ciclo di `foreach` esterno ottiene una riga alla volta dal file di dati. In questo caso, le righe sono definite da interruzioni di riga nel &#8212; file, ogni elemento di dati si trova su una riga a parte. Il ciclo esterno crea un nuovo elemento (`<li>` elemento) all'interno di un elenco ordinato (`<ol>` elemento).

    Il ciclo interno suddivide ogni riga di dati in elementi (campi) utilizzando una virgola come delimitatore. (In base all'esempio precedente, ciò significa che ogni riga contiene tre campi &#8212; il nome, il cognome e l'indirizzo di posta elettronica separati da una virgola). Il ciclo interno crea anche un elenco di `<ul>` e visualizza una voce di elenco per ogni campo nella riga di dati.

    Nel codice viene illustrato come utilizzare due tipi di dati, una matrice e il tipo di dati `char`. La matrice è obbligatoria perché il metodo `File.ReadAllLines` restituisce i dati sotto forma di matrice. Il tipo di dati `char` è obbligatorio perché il metodo `Split` restituisce un `array` in cui ogni elemento è di tipo `char`. Per informazioni sulle matrici, vedere [Introduzione alla programmazione Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).
3. Eseguire la pagina in un browser. Verranno visualizzati i dati immessi per gli esempi precedenti. 

    ![[immagine]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Visualizzazione di dati da un file delimitato da virgole di Microsoft Excel**
> 
> È possibile utilizzare Microsoft Excel per salvare i dati contenuti in un foglio di calcolo come file delimitati da virgole (file con*estensione CSV* ). Quando si esegue questa operazione, il file viene salvato in testo normale, non in formato Excel. Ogni riga del foglio di calcolo è separata da un'interruzioni di riga nel file di testo e ogni elemento di dati è separato da una virgola. È possibile usare il codice illustrato nell'esempio precedente per leggere un file delimitato da virgole di Excel semplicemente modificando il nome del file di dati nel codice.

<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Eliminazione di file

Per eliminare i file dal sito Web, è possibile usare il metodo `File.Delete`. Questa procedura illustra come consentire agli utenti di eliminare un'immagine (file con*estensione jpg* ) da una cartella *images* se conosce il nome del file.

> [!NOTE] 
> 
> **Importante** In un sito Web di produzione, in genere si limitano gli utenti autorizzati a apportare modifiche ai dati. Per informazioni su come configurare l'appartenenza e su come autorizzare gli utenti a eseguire attività sul sito, vedere [aggiunta di sicurezza e appartenenza a un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).

1. Nel sito Web creare una sottocartella denominata *images*.
2. Copiare uno o più file con *estensione jpg* nella cartella *images* .
3. Nella directory principale del sito Web creare un nuovo file denominato *filedelete. cshtml*.
4. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Questa pagina contiene un modulo in cui gli utenti possono immettere il nome di un file di immagine. Non entrano nell'estensione del nome di file *jpg* ; limitando il nome del file in questo modo, è possibile impedire agli utenti di eliminare file arbitrari nel sito.

    Il codice legge il nome file immesso dall'utente e quindi crea un percorso completo. Per creare il percorso, il codice usa il percorso del sito Web corrente (come restituito dal metodo `Server.MapPath`), il nome della cartella *images* , il nome fornito dall'utente e ". jpg" come stringa letterale.

    Per eliminare il file, il codice chiama il metodo `File.Delete`, passando il percorso completo appena costruito. Al termine del markup, il codice visualizza un messaggio di conferma che indica che il file è stato eliminato.
5. Eseguire la pagina in un browser. 

    ![[immagine]](working-with-files/_static/image5.jpg)
6. Immettere il nome del file da eliminare, quindi fare clic su **Invia**. Se il file è stato eliminato, il nome del file viene visualizzato nella parte inferiore della pagina.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Consentire agli utenti di caricare un file

Il `FileUpload` Helper consente agli utenti di caricare file nel sito Web. La procedura seguente illustra come consentire agli utenti di caricare un singolo file.

1. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è stato aggiunto in precedenza.
2. Nella cartella *App\_data* creare una nuova cartella e denominarla *nella UploadedFiles*.
3. Nella radice creare un nuovo file denominato *FileUpload. cshtml*.
4. Sostituire il contenuto esistente nella pagina con quanto segue: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    La parte relativa al corpo della pagina usa l'helper `FileUpload` per creare la casella di caricamento e i pulsanti che probabilmente si conosce:

    ![[immagine]](working-with-files/_static/image6.jpg)

    Le proprietà impostate per l'helper `FileUpload` specificano che si desidera una singola casella per il caricamento del file e che si desidera che il pulsante Invia legga il **caricamento**. Verranno aggiunte altre caselle più avanti in questo articolo.

    Quando l'utente fa clic su **carica**, il codice nella parte superiore della pagina ottiene il file e lo salva. L'oggetto `Request` usato in genere per ottenere i valori dai campi del modulo dispone anche di una matrice di `Files` che contiene il file o i file che sono stati caricati. È possibile ottenere singoli file dalle posizioni specifiche della matrice &#8212; , ad esempio per ottenere il primo file caricato, si ottiene `Request.Files[0]`, per ottenere il secondo file, si ottengono `Request.Files[1]`e così via. Tenere presente che nella programmazione il conteggio inizia in genere da zero.

    Quando si recupera un file caricato, lo si inserisce in una variabile (in questo caso, `uploadedFile`), in modo che sia possibile modificarla. Per determinare il nome del file caricato, è sufficiente ottenere la relativa proprietà `FileName`. Tuttavia, quando l'utente carica un file, `FileName` contiene il nome originale dell'utente, che include l'intero percorso. Potrebbe essere simile al seguente:

    *C:\Users\Public\Sample.txt*

    Tuttavia, non si desiderano tutte le informazioni sul percorso, perché si tratta del percorso nel computer dell'utente, non del server. Si vuole solo il nome del file effettivo (*Sample. txt*). È possibile rimuovere solo il file da un percorso usando il metodo `Path.GetFileName`, come indicato di seguito:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    L'oggetto `Path` è un'utilità che dispone di un numero di metodi come questo che è possibile utilizzare per rimuovere i percorsi, combinare i percorsi e così via.

    Una volta ottenuto il nome del file caricato, è possibile creare un nuovo percorso in cui si vuole archiviare il file caricato nel sito Web. In questo caso, si combinano `Server.MapPath`, i nomi delle cartelle (*App\_data/nella UploadedFiles*) e il nome del file appena rimosso per creare un nuovo percorso. È quindi possibile chiamare il metodo di `SaveAs` del file caricato per salvare effettivamente il file.
5. Eseguire la pagina in un browser. 

    ![[immagine]](working-with-files/_static/image7.jpg)
6. Fare clic su **Sfoglia** e quindi selezionare un file da caricare. 

    ![[immagine]](working-with-files/_static/image8.jpg)

    La casella di testo accanto al pulsante **Sfoglia** conterrà il percorso e il percorso del file.

    ![[immagine]](working-with-files/_static/image9.jpg)
7. Fare clic su **Upload**.
8. Nel sito Web fare clic con il pulsante destro del mouse sulla cartella del progetto, quindi scegliere **Aggiorna**.
9. Aprire la cartella *nella UploadedFiles* . Il file caricato si trova nella cartella. 

    ![[immagine]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Consentire agli utenti di caricare più file

Nell'esempio precedente si consente agli utenti di caricare un file. È tuttavia possibile utilizzare l'helper `FileUpload` per caricare più di un file alla volta. Questa operazione è utile per scenari come il caricamento di foto, in cui il caricamento di un file alla volta è noioso. Per informazioni sul caricamento di foto, vedere uso delle [Immagini in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897). Questo esempio Mostra come consentire agli utenti di caricare due per volta, anche se è possibile usare la stessa tecnica per caricare più di questo.

1. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato fatto.
2. Creare una nuova pagina denominata *FileUploadMultiple. cshtml*.
3. Sostituire il contenuto esistente nella pagina con quanto segue:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    In questo esempio, il `FileUpload` helper nel corpo della pagina è configurato in modo da consentire agli utenti di caricare due file per impostazione predefinita. Poiché `allowMoreFilesToBeAdded` è impostato su `true`, l'helper esegue il rendering di un collegamento che consente all'utente di aggiungere altre caselle di caricamento:

    ![[immagine]](working-with-files/_static/image11.jpg)

    Per elaborare i file caricati dall'utente, il codice usa la stessa tecnica di base usata nell'esempio &#8212; precedente per ottenere un file da `Request.Files` e quindi salvarlo. (Incluse le varie operazioni necessarie per ottenere il nome e il percorso del file corretti). Questa volta l'innovazione è che l'utente potrebbe caricare più file e non è noto a molti. Per scoprirlo, è possibile ottenere `Request.Files.Count`.

    Con questo numero a disposizione, è possibile scorrere in ciclo `Request.Files`, recuperare ogni file a sua volta e salvarlo. Quando si desidera eseguire il ciclo di un numero noto di volte tramite una raccolta, è possibile utilizzare un ciclo di `for`, come segue:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    La variabile `i` è semplicemente un contatore temporaneo che passerà da zero a qualsiasi limite superiore impostato. In questo caso, il limite superiore è il numero di file. Tuttavia, poiché il contatore inizia da zero, come avviene in genere per il conteggio degli scenari in ASP.NET, il limite superiore è in realtà uno inferiore al numero di file. Se vengono caricati tre file, il conteggio è pari a zero a 2.

    La variabile `uploadedCount` totali tutti i file caricati e salvati correttamente. Questo codice consente di tenere conto della possibilità che un file previsto non possa essere caricato.
4. Eseguire la pagina in un browser. Il browser Visualizza la pagina e le relative due caselle di caricamento.
5. Selezionare due file da caricare.
6. Fare clic su **Aggiungi un altro file**. Nella pagina viene visualizzata una nuova casella di caricamento. 

    ![[immagine]](working-with-files/_static/image12.jpg)
7. Fare clic su **Upload**.
8. Nel sito Web fare clic con il pulsante destro del mouse sulla cartella del progetto, quindi scegliere **Aggiorna**.
9. Aprire la cartella *nella UploadedFiles* per visualizzare i file caricati correttamente.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Utilizzo di immagini in un sito di Pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897)

[Esportazione in un file CSV](https://msdn.microsoft.com/library/ms155919.aspx)
