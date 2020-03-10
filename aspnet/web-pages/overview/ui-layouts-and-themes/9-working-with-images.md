---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Uso delle immagini in un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo capitolo illustra come aggiungere, visualizzare e modificare le immagini (ridimensionare, capovolgere e aggiungere filigrane) nel sito Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 53514b3c314fc182a43c82974ffcfa8158a636a1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631859"
---
# <a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Utilizzo di immagini in un sito di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come aggiungere, visualizzare e modificare le immagini (ridimensionare, capovolgere e aggiungere filigrane) in un sito Web Pagine Web ASP.NET (Razor).
> 
> Contenuto dell'esercitazione:
> 
> - Come aggiungere un'immagine a una pagina in modo dinamico.
> - Come consentire agli utenti di caricare un'immagine.
> - Come ridimensionare un'immagine.
> - Come capovolgere o ruotare un'immagine.
> - Come aggiungere una filigrana a un'immagine.
> - Come usare un'immagine come filigrana.
> 
> Queste sono le funzionalità di programmazione di ASP.NET introdotte nell'articolo:
> 
> - Helper `WebImage`.
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

<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Aggiunta dinamica di un'immagine a una pagina Web

È possibile aggiungere immagini al sito Web e a singole pagine durante lo sviluppo del sito Web. È anche possibile consentire agli utenti di caricare immagini, che potrebbero essere utili per le attività quali consentire l'aggiunta di una foto del profilo.

Se un'immagine è già disponibile nel sito e si vuole semplicemente visualizzarla in una pagina, usare un elemento HTML `<img>` come segue:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

In alcuni casi, tuttavia, è necessario essere in grado di visualizzare le &#8212; immagini in modo dinamico, ovvero non si conosce l'immagine da visualizzare finché la pagina non è in esecuzione.

La procedura descritta in questa sezione illustra come visualizzare un'immagine in tempo reale, in cui gli utenti specificano il nome del file di immagine da un elenco di nomi di immagine. Selezionano il nome dell'immagine da un elenco a discesa e quando inviano la pagina, viene visualizzata l'immagine selezionata.

![immagine](9-working-with-images/_static/image1.jpg "ch9images-1. jpg")

1. In WebMatrix creare un nuovo sito Web.
2. Aggiungere una nuova pagina denominata *DynamicImage. cshtml*.
3. Nella cartella radice del sito Web aggiungere una nuova cartella e denominarla *images*.
4. Aggiungere quattro immagini alla cartella *Immagini* appena creata. Tutte le immagini a cui si è pratici saranno disponibili, ma dovrebbero essere incluse in una pagina. Rinominare le immagini *Photo1. jpg*, *FOTO2. jpg*, *Photo3. jpg*e *Photo4. jpg*. In questa procedura non verrà usato *Photo4. jpg* , che verrà usato più avanti in questo articolo.
5. Verificare che le quattro immagini non siano contrassegnate come di sola lettura.
6. Sostituire il contenuto esistente nella pagina con quanto segue:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Il corpo della pagina ha un elenco a discesa (un elemento `<select>`) denominato `photoChoice`. L'elenco include tre opzioni e l'attributo `value` di ogni opzione di elenco ha il nome di una delle immagini inserite nella cartella *images* . In pratica, l'elenco consente all'utente di selezionare un nome descrittivo, ad esempio &quot;Photo 1&quot;, quindi passa il nome del file con *estensione jpg* quando viene inviata la pagina.

    Nel codice è possibile ottenere la selezione dell'utente (in altre parole, il nome del file di immagine) nell'elenco leggendo `Request["photoChoice"]`. Per prima cosa è possibile vedere se è presente una selezione. Se è presente, si costruisce un percorso per l'immagine costituito dal nome della cartella per le immagini e dal nome del file di immagine dell'utente. Se si è provato a costruire un percorso ma non c'era niente in `Request["photoChoice"]`, si otterrebbe un errore. In questo modo si ottiene un percorso relativo simile al seguente:

    *images/Photo1. jpg*

    Il percorso viene archiviato nella variabile denominata `imagePath` che sarà necessario più avanti nella pagina.

    Nel corpo è presente anche un elemento `<img>` usato per visualizzare l'immagine selezionata dall'utente. L'attributo `src` non è impostato su un nome file o un URL, come si farebbe per visualizzare un elemento statico. Al contrario, è impostato su `@imagePath`, ovvero ottiene il suo valore dal percorso impostato nel codice.

    La prima volta che la pagina viene eseguita, tuttavia, non è presente alcuna immagine da visualizzare perché l'utente non ha selezionato alcun elemento. Ciò significa in genere che l'attributo `src` sarà vuoto e che l'immagine verrebbe visualizzata come&quot; rossa &quot;x (o qualsiasi altro rendering del browser quando non è possibile trovare un'immagine). Per evitare questo problema, inserire l'elemento `<img>` in un blocco `if` per verificare se la variabile `imagePath` contiene elementi. Se l'utente ha effettuato una selezione, `imagePath` contiene il percorso. Se l'utente non ha selezionato un'immagine o se è la prima volta che la pagina viene visualizzata, non viene eseguito il rendering dell'elemento `<img>`.
7. Salvare il file ed eseguire la pagina in un browser. Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla.
8. Selezionare un'immagine dall'elenco a discesa, quindi fare clic su **immagine di esempio**. Assicurarsi di visualizzare diverse immagini per diverse scelte.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Caricamento di un'immagine

Nell'esempio precedente è stato illustrato come visualizzare un'immagine dinamicamente, ma funzionava solo con le immagini già presenti nel sito Web. Questa procedura illustra come consentire agli utenti di caricare un'immagine, che viene quindi visualizzata nella pagina. In ASP.NET è possibile modificare le immagini in tempo reale usando l'helper `WebImage`, che include metodi che consentono di creare, modificare e salvare immagini. L'helper `WebImage` supporta tutti i tipi di file di immagine Web comuni, tra cui *jpg*, *png*e *BMP*. In questo articolo si useranno le immagini *jpg* , ma è possibile usare qualsiasi tipo di immagine.

![immagine](9-working-with-images/_static/image2.jpg "ch9images-2. jpg")

1. Aggiungere una nuova pagina e denominarla *uploadimage. cshtml*.
2. Sostituire il contenuto esistente nella pagina con quanto segue: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Il corpo del testo dispone di un elemento `<input type="file">`, che consente agli utenti di selezionare un file da caricare. Quando si fa clic su **Invia**, il file selezionato viene inviato insieme al modulo.

    Per ottenere l'immagine caricata, usare l'helper `WebImage`, che include tutti i metodi utili per lavorare con le immagini. In particolare, si usa `WebImage.GetImageFromRequest` per ottenere l'immagine caricata (se presente) e archiviarla in una variabile denominata `photo`.

    Numerose operazioni in questo esempio includono il recupero e l'impostazione dei nomi di file e percorsi. Il problema è che si desidera ottenere il nome (e solo il nome) dell'immagine caricata dall'utente, quindi creare un nuovo percorso in cui archiviare l'immagine. Poiché gli utenti potrebbero caricare più immagini con lo stesso nome, è possibile usare un po' di codice aggiuntivo per creare nomi univoci e assicurarsi che gli utenti non sovrascrivano le immagini esistenti.

    Se un'immagine è stata effettivamente caricata (test `if (photo != null)`), il nome dell'immagine viene ottenuto dalla proprietà `FileName` dell'immagine. Quando l'utente carica l'immagine, `FileName` contiene il nome originale dell'utente, che include il percorso dal computer dell'utente. Potrebbe essere simile al seguente:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Non si desiderano tutte le informazioni sul percorso &#8212; , anche se si desidera solo il nome del file effettivo (*SamplePhoto1. jpg*). È possibile rimuovere solo il file da un percorso usando il metodo `Path.GetFileName`, come indicato di seguito:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Si crea quindi un nuovo nome file univoco aggiungendo un GUID al nome originale. Per ulteriori informazioni sui GUID, vedere [informazioni sui GUID](#SB_AboutGUIDs) più avanti in questo articolo. Quindi si costruisce un percorso completo che è possibile usare per salvare l'immagine. Il percorso di salvataggio è costituito dal nome del nuovo file, dalla cartella (immagini) e dal percorso del sito Web corrente.

    > [!NOTE]
    > Per consentire al codice di salvare i file nella cartella *images* , l'applicazione deve disporre delle autorizzazioni di lettura/scrittura per la cartella. Nel computer di sviluppo non si tratta in genere di un problema. Tuttavia, quando si pubblica il sito in un server Web del provider di hosting, potrebbe essere necessario impostare in modo esplicito tali autorizzazioni. Se si esegue questo codice nel server di un provider di hosting e si ottengono errori, rivolgersi al provider di hosting per scoprire come impostare tali autorizzazioni.

    Infine, si passa il percorso di salvataggio al metodo `Save` dell'helper `WebImage`. Questa operazione archivia l'immagine caricata con il nuovo nome. Il metodo Save è simile al seguente: `photo.Save(@"~\" + imagePath)`. Il percorso completo viene aggiunto alla `@"~\"`, ovvero il percorso del sito Web corrente. Per informazioni sull'operatore `~`, vedere [Introduzione alla programmazione Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).

    Come nell'esempio precedente, il corpo della pagina contiene un elemento `<img>` per visualizzare l'immagine. Se `imagePath` è stato impostato, viene eseguito il rendering dell'elemento `<img>` e il relativo attributo `src` è impostato sul valore di `imagePath`.
3. Eseguire la pagina in un browser.
4. Caricare un'immagine e verificare che venga visualizzata nella pagina.
5. Nel sito aprire la cartella *Immagini* . Si noterà che è stato aggiunto un nuovo file il cui nome file è simile al seguente: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_Photo. png*

    Si tratta dell'immagine caricata con un GUID aggiunto al nome. (Il proprio file avrà un GUID diverso e probabilmente verrà denominato un valore diverso da *Photo. png*).

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Informazioni sui GUID
> 
> Un GUID (ID univoco globale) è un identificatore di cui viene in genere eseguito il rendering in un formato simile al seguente: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. I numeri e le lettere (da A-F) sono diversi per ogni GUID, ma tutti seguono il modello di utilizzo di gruppi di 8-4-4-4-12 caratteri. Tecnicamente, un GUID è un numero a 16 byte/128 bit. Quando è necessario un GUID, è possibile chiamare codice specializzato che genera automaticamente un GUID. L'idea alla base dei GUID è che tra le dimensioni enormi del numero (3,4 x 10<sup>38</sup>) e l'algoritmo per la relativa generazione, il numero risultante è quasi sicuramente uno dei tipi. I GUID rappresentano quindi un modo efficace per generare nomi per gli elementi quando è necessario garantire che non venga usato due volte lo stesso nome. Il lato negativo, ovviamente, è che i GUID non sono particolarmente intuitivi, quindi tendono a essere usati quando il nome viene usato solo nel codice.

<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Ridimensionamento di un'immagine

Se il sito Web accetta immagini da un utente, potrebbe essere necessario ridimensionarle prima di visualizzarle o salvarle. Per questa operazione è possibile usare di nuovo l'helper `WebImage`.

Questa procedura illustra come ridimensionare un'immagine caricata per creare un'anteprima e quindi salvare l'anteprima e l'immagine originale nel sito Web. È possibile visualizzare l'anteprima nella pagina e usare un collegamento ipertestuale per reindirizzare gli utenti all'immagine a dimensione intera.

![immagine](9-working-with-images/_static/image3.jpg "ch9images-3. jpg")

1. Aggiungere una nuova pagina denominata *thumbnail. cshtml*.
2. Nella cartella *Immagini* creare una sottocartella denominata *thumbs*.
3. Sostituire il contenuto esistente nella pagina con quanto segue: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Questo codice è simile al codice dell'esempio precedente. La differenza è che questo codice salva l'immagine due volte, una volta normalmente e una volta dopo la creazione di una copia di anteprima dell'immagine. Per prima cosa, è possibile ottenere l'immagine caricata e salvarla nella cartella *images* . Si crea quindi un nuovo percorso per l'immagine di anteprima. Per creare effettivamente l'anteprima, chiamare il metodo di `Resize` dell'helper `WebImage` per creare un'immagine di 60 pixel per 60-pixel. Nell'esempio viene illustrato come mantenere le proporzioni e come impedire che l'immagine venga ingrandita (se le nuove dimensioni rendono effettivamente più grande l'immagine). L'immagine ridimensionata viene quindi salvata nella sottocartella *thumbs* .

    Alla fine del markup, si usa lo stesso elemento `<img>` con l'attributo Dynamic `src`, illustrato negli esempi precedenti, per visualizzare l'immagine in modo condizionale. In questo caso, viene visualizzata l'anteprima. Si usa anche un elemento `<a>` per creare un collegamento ipertestuale alla versione principale dell'immagine. Come per l'attributo `src` dell'elemento `<img>`, è necessario impostare dinamicamente l'attributo `href` dell'elemento `<a>` su quello che si trova in `imagePath`. Per assicurarsi che il percorso possa funzionare come un URL, passare `imagePath` al metodo `Html.AttributeEncode`, che converte i caratteri riservati nel percorso in caratteri che sono OK in un URL.
4. Eseguire la pagina in un browser.
5. Caricare una foto e verificare che l'anteprima sia visualizzata.
6. Fare clic sull'anteprima per visualizzare l'immagine a dimensione intera.
7. Nelle *Immagini* e nelle *Immagini/thumbs*, si noti che sono stati aggiunti nuovi file.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Rotazione e capovolgimento di un'immagine

Il `WebImage` Helper consente inoltre di capovolgere e ruotare le immagini. Questa procedura Mostra come ottenere un'immagine dal server, capovolgerla (verticalmente), salvarla e visualizzare l'immagine capovolta nella pagina. In questo esempio si sta usando solo un file già presente nel server (*FOTO2. jpg*). In un'applicazione reale, si potrebbe probabilmente capovolgere un'immagine il cui nome si ottiene in modo dinamico, come negli esempi precedenti.

![immagine](9-working-with-images/_static/image4.jpg "ch9images-4. jpg")

1. Aggiungere una nuova pagina denominata *FlipImage. cshtml*.
2. Sostituire il contenuto esistente nella pagina con quanto segue: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Il codice usa l'helper `WebImage` per ottenere un'immagine dal server. Si crea il percorso dell'immagine usando la stessa tecnica usata negli esempi precedenti per salvare le immagini e si passa tale percorso quando si crea un'immagine usando `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Se viene trovata un'immagine, è possibile creare un nuovo percorso e un nuovo nome di file, come negli esempi precedenti. Per capovolgere l'immagine, chiamare il metodo `FlipVertical` e quindi salvare nuovamente l'immagine.

    L'immagine viene nuovamente visualizzata nella pagina utilizzando l'elemento `<img>` con l'attributo `src` impostato su `imagePath`.
3. Eseguire la pagina in un browser. L'immagine per *FOTO2. jpg* viene visualizzata in basso.
4. Aggiornare la pagina o richiedere di nuovo la pagina per visualizzare di nuovo il pulsante destro del mouse sull'immagine.

Per ruotare un'immagine, si usa lo stesso codice, ad eccezione del fatto che anziché chiamare il `FlipVertical` o `FlipHorizontal`, si chiama `RotateLeft` o `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Aggiunta di una filigrana a un'immagine

Quando si aggiungono immagini al sito Web, potrebbe essere necessario aggiungere una filigrana all'immagine prima di salvarla o visualizzarla in una pagina. Spesso gli utenti usano le filigrane per aggiungere informazioni sul copyright a un'immagine o per pubblicizzare il nome dell'azienda.

![immagine](9-working-with-images/_static/image5.jpg "ch9images-5. jpg")

1. Aggiungere una nuova pagina denominata *Watermark. cshtml*.
2. Sostituire il contenuto esistente nella pagina con quanto segue: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Questo codice è simile al codice nella pagina *FlipImage. cshtml* precedente (anche se questa volta usa il file *Photo3. jpg* ). Per aggiungere la filigrana, chiamare il metodo di `AddTextWatermark` dell'helper `WebImage` prima di salvare l'immagine. Nella chiamata a `AddTextWatermark`si passa il testo &quot;&quot;filigrana, si imposta il colore del carattere su giallo e si imposta la famiglia di caratteri su Arial. Anche se non è riportato qui, l'helper `WebImage` consente anche di specificare l'opacità, la famiglia di caratteri e le dimensioni del carattere e la posizione del testo della filigrana. Quando si salva l'immagine, non deve essere di sola lettura.

    Come è stato illustrato in precedenza, l'immagine viene visualizzata nella pagina usando l'elemento `<img>` con l'attributo SRC impostato su `@imagePath`.
3. Eseguire la pagina in un browser. Si noti il testo "My Watermark" nell'angolo in basso a destra dell'immagine.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Uso di un'immagine come filigrana

Invece di usare il testo per una filigrana, è possibile usare un'altra immagine. A volte le persone usano immagini come un logo aziendale come filigrana oppure usano un'immagine di filigrana invece del testo per ottenere informazioni sul copyright.

![immagine](9-working-with-images/_static/image6.jpg "ch9images-6. jpg")

1. Aggiungere una nuova pagina denominata *ImageWatermark. cshtml*.
2. Aggiungere un'immagine alla cartella *images* che è possibile usare come logo e rinominare l'immagine *MyCompanyLogo. jpg*. Questa immagine deve essere un'immagine che è possibile vedere chiaramente quando è impostata su 80 pixel di larghezza e 20 pixel di altezza.
3. Sostituire il contenuto esistente nella pagina con quanto segue: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Si tratta di un'altra variante del codice degli esempi precedenti. In questo caso, chiamare `AddImageWatermark` per aggiungere l'immagine della filigrana all'immagine di destinazione (*Photo3. jpg*) prima di salvare l'immagine. Quando si chiama `AddImageWatermark`, si imposta la larghezza su 80 pixel e l'altezza su 20 pixel. L'immagine *MyCompanyLogo. jpg* è allineata orizzontalmente al centro e allineata verticalmente nella parte inferiore dell'immagine di destinazione. L'opacità è impostata su 100% e la spaziatura interna è impostata su 10 pixel. Se l'immagine della filigrana è maggiore di quella di destinazione, non verrà eseguita alcuna operazione. Se l'immagine della filigrana è più grande dell'immagine di destinazione e si imposta la spaziatura interna per la filigrana dell'immagine su zero, la filigrana viene ignorata.

    Come prima, è possibile visualizzare l'immagine usando l'elemento `<img>` e un attributo Dynamic `src`.
4. Eseguire la pagina in un browser. Si noti che l'immagine della filigrana viene visualizzata nella parte inferiore dell'immagine principale.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Uso dei file in un sito di Pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202896)

[Introduzione alla programmazione Pagine Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
