---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Usare i controller e le visualizzazioni per implementare un'interfaccia utente di elenco/dettagli | Microsoft Docs
author: microsoft
description: Il passaggio 4 Mostra come aggiungere un controller all'applicazione che sfrutta i vantaggi del modello per fornire agli utenti un'esperienza di navigazione in elenco di dati/dettagli...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 74319fe5ea4c79b50140834349e2fdf86420cfbb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600744"
---
# <a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usare controller e visualizzazioni per implementare un'interfaccia utente elenco/dettagli

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 4 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.
> 
> Il passaggio 4 Mostra come aggiungere un controller all'applicazione che sfrutta i vantaggi del modello per offrire agli utenti un'esperienza di visualizzazione di elenchi di dati/dettagli per le cene nel sito di NerdDinner.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner passaggio 4: controller e visualizzazioni

Con i framework Web tradizionali (ASP classico, PHP, Web Form ASP.NET e così via), gli URL in ingresso vengono in genere mappati a file su disco. Ad esempio, una richiesta per un URL come "/Products.aspx" o "/Products.php" potrebbe essere elaborata da un file "Products. aspx" o "Products. php".

I framework MVC basati sul Web mappano gli URL al codice server in modo leggermente diverso. Anziché eseguire il mapping degli URL in ingresso ai file, eseguono invece il mapping degli URL ai metodi nelle classi. Queste classi sono denominate "controller" e sono responsabili dell'elaborazione delle richieste HTTP in ingresso, della gestione dell'input dell'utente, del recupero e del salvataggio dei dati e della determinazione della risposta da inviare al client (visualizzazione HTML, download di un file, Reindirizzamento a un altro URL e così via.

Ora che è stato creato un modello di base per l'applicazione NerdDinner, il passaggio successivo consiste nell'aggiunta di un controller all'applicazione che ne sfrutta i vantaggi per fornire agli utenti un'esperienza di navigazione dei dati/dettagli per le cene sul sito.

### <a name="adding-a-dinnerscontroller-controller"></a>Aggiunta di un controller DinnersController

Si inizierà facendo clic con il pulsante destro del mouse sulla cartella "Controllers" all'interno del progetto Web e quindi scegliendo il comando di menu **Add-&gt;controller** (è anche possibile eseguire questo comando digitando CTRL + M, CTRL + C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Verrà visualizzata la finestra di dialogo "Aggiungi controller":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Denominare il nuovo controller "DinnersController" e fare clic sul pulsante "Aggiungi". In Visual Studio verrà quindi aggiunto un file DinnersController.cs nella directory \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Verrà inoltre aperta la nuova classe DinnersController all'interno dell'editor di codice.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Aggiunta di metodi di azione index () e Details () alla classe DinnersController

Si desidera consentire ai visitatori che usano l'applicazione di sfogliare un elenco di cene imminenti e consentire loro di fare clic su qualsiasi cena nell'elenco per visualizzare dettagli specifici. A tale scopo, pubblicare gli URL seguenti dall'applicazione:

| **URL** | **Scopo** |
| --- | --- |
| *Cene* | Visualizza un elenco HTML di cene imminenti |
| */Dinners/Details/[ID]* | Consente di visualizzare i dettagli relativi a una specifica cena indicata da un parametro "ID" incorporato nell'URL, che corrisponderà alla DinnerID della cena nel database. Ad esempio:/Dinners/Details/2 Visualizza una pagina HTML con i dettagli relativi alla cena il cui valore DinnerID è 2. |

Si pubblicheranno le implementazioni iniziali di questi URL aggiungendo due "metodi di azione" pubblici alla classe DinnersController, come indicato di seguito:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Si eseguirà quindi l'applicazione NerdDinner e si userà il browser per richiamarli. Se si digita l'URL *"/dinners/"* , verrà eseguito il metodo *index ()* e verrà restituita la risposta seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Digitando l'URL *"/dinners/Details/2"* , il metodo *Details ()* verrà eseguito e restituirà la risposta seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Ci si potrebbe chiedere: in che modo ASP.NET MVC sapeva creare la classe DinnersController e richiamare tali metodi? Per comprendere in modo approfondito il funzionamento del routing.

### <a name="understanding-aspnet-mvc-routing"></a>Informazioni sul routing di ASP.NET MVC

ASP.NET MVC include un potente motore di routing degli URL che offre una notevole flessibilità nel controllo della modalità di mapping degli URL alle classi controller. Consente di personalizzare completamente il modo in cui ASP.NET MVC sceglie la classe del controller da creare, il metodo da richiamare su di esso, oltre a configurare diversi modi in cui le variabili possono essere analizzate automaticamente da URL/querystring e passate al metodo come argomenti di parametro. Offre la flessibilità necessaria per ottimizzare completamente un sito per la SEO (ottimizzazione del motore di ricerca), nonché per pubblicare qualsiasi struttura URL desiderata da un'applicazione.

Per impostazione predefinita, i nuovi progetti MVC ASP.NET sono dotati di un set preconfigurato di regole di routing degli URL già registrate. Questo consente di iniziare facilmente a usare un'applicazione senza dover configurare in modo esplicito qualsiasi elemento. Le registrazioni predefinite della regola di routing sono disponibili nella classe "applicazione" dei progetti, che è possibile aprire facendo doppio clic sul file "Global. asax" nella radice del progetto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Le regole di routing predefinite di ASP.NET MVC vengono registrate nel metodo "RegisterRoutes" di questa classe:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"Route. MapRoute () "la chiamata al metodo precedente registra una regola di routing predefinita che esegue il mapping degli URL in ingresso alle classi controller usando il formato URL:"/{Controller}/{Action}/{ID} "– dove" controller "è il nome della classe controller di cui creare un'istanza," Action "è il nome di un metodo pubblico da richiamare su di esso e" ID "è un parametro facoltativo incorporato nell'URL che può essere passato come argomento al metodo. Il terzo parametro passato alla chiamata al metodo "MapRoute ()" è un set di valori predefiniti da usare per i valori controller/azione/ID nel caso in cui non siano presenti nell'URL (controller = "Home", Action = "index", ID = "").

Di seguito è riportata una tabella in cui viene illustrato come viene eseguito il mapping di una varietà di URL utilizzando la regola di route predefinita "<em>/{Controllers}/{Action}/{ID}"</em>:

| **URL** | **Classe controller** | **Metodo di azione** | **Parametri passati** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Dettagli (ID) | id=2 |
| */Dinners/Edit/5* | DinnersController | Modifica (ID) | id=5 |
| */Dinners/Create* | DinnersController | Create() | N/D |
| */Dinners* | DinnersController | Index () | N/D |
| */Home* | HomeController | Index () | N/D |
| */* | HomeController | Index () | N/D |

Le ultime tre righe mostrano i valori predefiniti (controller = Home, action = index, ID = "") in uso. Poiché il metodo "index" viene registrato come nome di azione predefinito se non ne viene specificato uno, gli URL "/dinners" e "/Home" causano il richiamo del metodo di azione index () sulle classi controller. Poiché il controller "Home" viene registrato come controller predefinito, se non ne viene specificato uno, l'URL "/" causa la creazione del HomeController e il metodo di azione index () su di esso per essere richiamato.

Se non si è interessati a queste regole predefinite di routing degli URL, la novità è che sono facili da modificare, ma è sufficiente modificarle all'interno del metodo RegisterRoutes. Per l'applicazione NerdDinner, tuttavia, non verranno modificate le regole di routing degli URL predefinite, ma verranno usate solo così come sono.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Uso di DinnerRepository dalla DinnersController

A questo punto, sostituire l'implementazione corrente dei metodi di azione index () e Details () di DinnersController con implementazioni che usano il modello.

Verrà usata la classe DinnerRepository creata in precedenza per implementare il comportamento. Per iniziare, aggiungere un'istruzione "using" che fa riferimento allo spazio dei nomi "NerdDinner. Models" e quindi dichiarare un'istanza di DinnerRepository come campo nella classe DinnerController.

Più avanti in questo capitolo verrà introdotto il concetto di "inserimento delle dipendenze" e verrà illustrato un altro modo per i controller di ottenere un riferimento a un DinnerRepository che consente un migliore testing unità, ma per il momento verrà creata solo un'istanza del DinnerRepository come indicato di seguito.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

A questo punto è possibile generare una risposta HTML usando gli oggetti del modello di dati recuperati.

### <a name="using-views-with-our-controller"></a>Uso delle visualizzazioni con il controller

Sebbene sia possibile scrivere codice all'interno dei metodi di azione per assemblare il codice HTML e quindi usare il metodo helper *Response. Write ()* per inviarlo di nuovo al client, questo approccio diventa abbastanza semplice da gestire. Un approccio molto più efficace consiste nell'eseguire solo la logica dell'applicazione e dei dati all'interno dei metodi di azione DinnersController e di passare i dati necessari per eseguire il rendering di una risposta HTML in un modello "View" separato responsabile dell'output della rappresentazione HTML di esso. Come si vedrà in un secondo momento, un modello "View" è un file di testo che in genere contiene una combinazione di markup HTML e codice di rendering incorporato.

La separazione della logica del controller dal rendering della visualizzazione offre diversi vantaggi. In particolare, consente di applicare una chiara "separazione dei problemi" tra il codice dell'applicazione e l'interfaccia utente/codice di rendering. In questo modo è molto più semplice testare unit test della logica dell'applicazione in isolamento dalla logica di rendering dell'interfaccia utente. Consente di modificare più facilmente i modelli di rendering dell'interfaccia utente senza dover apportare modifiche al codice dell'applicazione. E può semplificare la collaborazione tra gli sviluppatori e i progettisti nei progetti.

È possibile aggiornare la classe DinnersController per indicare che si vuole usare un modello di vista per restituire una risposta dell'interfaccia utente HTML modificando le firme del metodo dei due metodi di azione da avere un tipo restituito "void" per avere invece un tipo restituito "ActionResult". È quindi possibile chiamare il metodo helper *View ()* sulla classe di base controller per restituire un oggetto "ViewResult" come riportato di seguito:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La firma del metodo helper *View ()* usato in precedenza è simile alla seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Il primo parametro del metodo helper *View ()* è il nome del file modello di visualizzazione che si vuole usare per eseguire il rendering della risposta HTML. Il secondo parametro è un oggetto modello che contiene i dati necessari al modello di visualizzazione per eseguire il rendering della risposta HTML.

All'interno del metodo di azione index () viene chiamato il metodo helper *View ()* e viene indicato che si vuole eseguire il rendering di un elenco HTML di cene usando un modello di visualizzazione "index". Viene passato il modello di visualizzazione una sequenza di oggetti Dinner da cui generare l'elenco:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

All'interno del metodo di azione Details () si tenta di recuperare un oggetto Dinner usando l'ID fornito nell'URL. Se viene trovata una cena valida, viene chiamato il metodo helper *View ()* , che indica che si vuole usare un modello di visualizzazione "Details" per eseguire il rendering dell'oggetto Dinner recuperato. Se viene richiesta una cena non valida, viene visualizzato un messaggio di errore utile che indica che la cena non esiste usando un modello di visualizzazione "NotFound" (e una versione di overload del metodo helper *View ()* che accetta solo il nome del modello):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Verranno ora implementati i modelli di visualizzazione "NotFound", "Details" e "index".

### <a name="implementing-the-notfound-view-template"></a>Implementazione del modello di visualizzazione "NotFound"

Si inizierà con l'implementazione del modello di visualizzazione "NotFound", che visualizza un messaggio di errore descrittivo che indica che non è possibile trovare la cena richiesta.

Verrà creato un nuovo modello di vista posizionando il cursore del testo all'interno di un metodo di azione del controller, quindi facendo clic con il pulsante destro del mouse e scegliendo il comando di menu "Aggiungi visualizzazione" (è anche possibile eseguire questo comando digitando CTRL + M, CTRL + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Verrà visualizzata una finestra di dialogo "Aggiungi visualizzazione", come indicato di seguito. Per impostazione predefinita, nella finestra di dialogo viene prepopolato il nome della vista da creare in modo che corrisponda al nome del metodo di azione in cui si trovava il cursore all'avvio della finestra di dialogo (in questo caso "dettagli"). Poiché si vuole implementare prima il modello "NotFound", si eseguirà l'override di questo nome di visualizzazione e lo si imposterà su "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo modello di visualizzazione "NotFound. aspx" nella directory "\Views\Dinners", che verrà creato anche se la directory non esiste già:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Verrà inoltre aperto il nuovo modello di visualizzazione "NotFound. aspx" nell'editor del codice:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Per impostazione predefinita, i modelli di visualizzazione hanno due "aree di contenuto" in cui è possibile aggiungere contenuto e codice. Il primo consente di personalizzare il "titolo" della pagina HTML restituita. Il secondo consente di personalizzare il "contenuto principale" della pagina HTML restituita.

Per implementare il modello di visualizzazione "NotFound", verrà aggiunto il contenuto di base:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Possiamo quindi provarlo all'interno del browser. A tale scopo, è necessario richiedere l'URL *"/dinners/Details/9999"* . In questo modo si farà riferimento a una cena che attualmente non esiste nel database e il metodo di azione DinnersController. Details () eseguirà il rendering del modello di visualizzazione "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Una cosa che noterete nella schermata precedente è che il modello di visualizzazione di base ha ereditato una serie di codice HTML che racchiude il contenuto principale sullo schermo. Questo perché il modello di visualizzazione usa un modello "pagina master" che consente di applicare un layout coerente in tutte le visualizzazioni nel sito. In una parte successiva di questa esercitazione verrà illustrata la modalità di lavoro delle pagine master.

### <a name="implementing-the-details-view-template"></a>Implementazione del modello di visualizzazione "dettagli"

A questo punto, è possibile implementare il modello di visualizzazione "dettagli", che genera il codice HTML per un singolo modello dinner.

A tale scopo, posizionare il cursore di testo all'interno del metodo di azione dettagli, quindi fare clic con il pulsante destro del mouse e scegliere il comando di menu "Aggiungi visualizzazione" oppure premere CTRL + M, CTRL + V:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione". Si manterrà il nome della visualizzazione predefinita ("Details"). Si selezionerà anche la casella di controllo "crea una visualizzazione fortemente tipizzata" nella finestra di dialogo e si seleziona (usando l'elenco a discesa ComboBox) il nome del tipo di modello che viene passato dal controller alla visualizzazione. Per questa visualizzazione viene passato un oggetto Dinner (il nome completo di questo tipo è: "NerdDinner. Models. Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

A differenza del modello precedente, in cui si è scelto di creare una "visualizzazione vuota", questa volta si sceglierà "impalcatura" automatica della visualizzazione usando un modello "dettagli". È possibile indicarlo modificando l'elenco a discesa "Visualizza contenuto" nella finestra di dialogo sopra.

"Impalcature" genera un'implementazione iniziale del modello di visualizzazione dettagli in base all'oggetto Dinner a cui viene passato. Si tratta di un modo semplice per iniziare rapidamente a usare l'implementazione del modello di visualizzazione.

Quando si fa clic sul pulsante "Aggiungi", in Visual Studio viene creato un nuovo file di modello di visualizzazione "Details. aspx" nella directory "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Verrà inoltre aperto il nuovo modello di visualizzazione "Details. aspx" nell'editor di codice. Conterrà un'implementazione iniziale di un patibolo di una visualizzazione dettagli basata su un modello dinner. Il motore di ponteggi usa la reflection .NET per esaminare le proprietà pubbliche esposte sulla classe passata e aggiungere il contenuto appropriato in base a ogni tipo trovato:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

È possibile richiedere l'URL *"/dinners/Details/1"* per vedere l'aspetto dell'implementazione dell'impalcatura "Details" nel browser. Con questo URL verrà visualizzata una delle cene aggiunte manualmente al database al momento della creazione:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Questa operazione ci consente di iniziare rapidamente e fornisce un'implementazione iniziale della vista Details. aspx. Possiamo quindi modificarlo per personalizzare l'interfaccia utente con la soddisfazione.

Quando si esamina il modello details. aspx in modo più accurato, si noterà che contiene HTML statico e codice di rendering incorporato. &lt;%%&gt; codice Nuggets eseguono il codice quando viene eseguito il rendering del modello di visualizzazione e &lt;% =%&gt; codice Nuggets eseguono il codice contenuto al suo interno e quindi eseguono il rendering del risultato nel flusso di output del modello.

È possibile scrivere codice all'interno della vista che accede all'oggetto modello "Dinner" passato dal controller usando una proprietà "Model" fortemente tipizzata. Visual Studio fornisce il codice completo-IntelliSense quando si accede a questa proprietà "Model" nell'Editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Apportare alcune modifiche in modo che l'origine del modello di visualizzazione dettagli finale sia simile alla seguente:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Quando si accede di nuovo all'URL *"/dinners/Details/1"* , il rendering viene eseguito come segue:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementazione del modello di visualizzazione "index"

A questo punto, è possibile implementare il modello di visualizzazione "index", che genera un elenco di cene imminenti. A tale scopo, si posiziona il cursore del testo all'interno del metodo di azione dell'indice, quindi si fa clic con il pulsante destro del mouse e si sceglie il comando di menu "Aggiungi visualizzazione" oppure si preme CTRL + M, CTRL + V.

Nella finestra di dialogo "Aggiungi visualizzazione" si manterrà il modello di visualizzazione denominato "index" e si seleziona la casella di controllo "crea una visualizzazione fortemente tipizzata". Questa volta si sceglierà di generare automaticamente un modello di visualizzazione "list" e di selezionare "NerdDinner. Models. Dinner" come tipo di modello passato alla visualizzazione (il che perché abbiamo indicato che è in corso la creazione di un "list", la finestra di dialogo Aggiungi visualizzazione presuppone che ci sia passaggio di una sequenza di oggetti dinner dal controller alla visualizzazione:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Quando si fa clic sul pulsante "Aggiungi", in Visual Studio viene creato un nuovo file di modello di visualizzazione "index. aspx" nella directory "\Views\Dinners". Verrà "impalcatura" un'implementazione iniziale al suo interno che fornisce un elenco di tabelle HTML delle cene passate alla visualizzazione.

Quando si esegue l'applicazione e si accede all'URL *"/dinners/"* , verrà eseguito il rendering dell'elenco di cene come segue:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La soluzione Table precedente fornisce un layout simile a Grid dei nostri dati di Dinner, che non è quello che vogliamo per i nostri clienti che rimandano a cena. È possibile aggiornare il modello di visualizzazione index. aspx e modificarlo per elencare un minor numero di colonne di dati e usare un &lt;UL&gt; elemento per eseguirne il rendering invece di una tabella usando il codice seguente:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Nell'istruzione foreach precedente viene utilizzata la parola chiave "var" mentre viene effettuato il ciclo su ogni cena del modello. Chi non ha familiarità con 3,0 potrebbe pensare che l'uso di "var" significa che l'oggetto Dinner è ad C# associazione tardiva. Significa invece che il compilatore usa l'inferenza del tipo per la proprietà "Model" fortemente tipizzata (che è di tipo "IEnumerable&lt;Dinner&gt;") e la compilazione della variabile locale "Dinner" come tipo Dinner, ovvero otteniamo IntelliSense completo e il controllo in fase di compilazione all'interno dei blocchi di codice:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Quando si raggiunge l'aggiornamento sull'URL */dinners* nel browser, la vista aggiornata ora è simile alla seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Si tratta di un aspetto migliore, ma non è ancora completamente presente. L'ultimo passaggio consiste nel consentire agli utenti finali di fare clic su cene individuali nell'elenco e visualizzarne i dettagli. Questa operazione verrà implementata tramite il rendering degli elementi Hyperlink HTML che si collegano al metodo di azione dettagli nel DinnersController.

È possibile generare questi collegamenti ipertestuali all'interno della vista index in uno dei due modi seguenti. Il primo consiste nel creare manualmente il codice HTML &lt;un&gt; elementi come riportato di seguito, in cui vengono incorporati &lt;%%&gt; blocchi all'interno del &lt;un elemento HTML&gt;:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Un approccio alternativo è quello di sfruttare il metodo helper "HTML. ActionLink ()" integrato all'interno di ASP.NET MVC che supporta la creazione a livello di codice di un HTML &lt;un elemento&gt; che si collega a un altro metodo di azione in un controller:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Primo parametro del codice HTML. il metodo helper ActionLink () è il testo del collegamento da visualizzare, in questo caso il titolo della cena, il secondo parametro è il nome dell'azione del controller a cui si vuole generare il collegamento (in questo caso il metodo Details) e il terzo parametro è un set di parametri da inviare all'azione (implementato come tipo anonimo con nome/valori di proprietà). In questo caso viene specificato il parametro "ID" della cena a cui ci si vuole collegare e perché la regola di routing degli URL predefinita in ASP.NET MVC è "{Controller}/{Action}/{id}" il metodo helper HTML. ActionLink () genererà l'output seguente:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Per la vista index. aspx verrà usato l'approccio del metodo helper HTML. ActionLink () e ogni cena nell'elenco verrà collegato all'URL dei dettagli appropriato:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Ora, quando si raggiunge l'URL */dinners* , l'elenco di cene è simile al seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Quando si fa clic su una delle cene nell'elenco, si passerà a visualizzarne i dettagli:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Denominazione basata sulle convenzioni e struttura di directory \Views

Per impostazione predefinita, le applicazioni MVC ASP.NET utilizzano una struttura di denominazione di directory basata sulle convenzioni durante la risoluzione dei modelli di visualizzazione. Ciò consente agli sviluppatori di evitare di dover qualificare completamente il percorso di un percorso quando si fa riferimento alle viste all'interno di una classe controller. Per impostazione predefinita, MVC ASP.NET cercherà il file di modello di visualizzazione nella directory * \Views\[controllerName]\* sotto l'applicazione.

Ad esempio, abbiamo lavorato alla classe DinnersController, che fa riferimento in modo esplicito a tre modelli di visualizzazione: "index", "Details" e "NotFound". Per impostazione predefinita, MVC ASP.NET cercherà queste viste nella directory *\Views\Dinners* sotto la directory radice dell'applicazione:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Si noti sopra come sono attualmente presenti tre classi controller all'interno del progetto (DinnersController, HomeController e AccountController: le ultime due sono state aggiunte per impostazione predefinita al momento della creazione del progetto) e sono presenti tre sottodirectory (una per ogni controller) nella directory \Views

Le visualizzazioni a cui fanno riferimento i controller Home e Accounts risolveranno automaticamente i modelli di visualizzazione dalle rispettive directory *\Views\Home* e *\Views\Account* . La sottodirectory *\Views\Shared* consente di archiviare i modelli di visualizzazione che vengono riutilizzati in più controller all'interno dell'applicazione. Quando MVC ASP.NET tenta di risolvere un modello di visualizzazione, viene innanzitutto verificato all'interno della directory specifica *\Views\[controller]* e, se non riesce a trovare il modello di visualizzazione, verrà visualizzato nella directory *\Views\Shared* .

Per la denominazione dei singoli modelli di visualizzazione, è consigliabile che il modello di visualizzazione condivida lo stesso nome del metodo di azione che ne ha causato il rendering. Ad esempio, sopra il metodo di azione "index" si usa la visualizzazione "index" per eseguire il rendering del risultato della visualizzazione e il metodo di azione "Details" usa la visualizzazione "Details" per eseguire il rendering dei risultati. Questo consente di individuare rapidamente il modello associato a ciascuna azione.

Gli sviluppatori non devono specificare in modo esplicito il nome del modello di visualizzazione quando il modello di vista ha lo stesso nome del metodo di azione richiamato sul controller. È invece possibile passare semplicemente l'oggetto modello al metodo helper "View ()" (senza specificare il nome della visualizzazione) e ASP.NET MVC dedurrà automaticamente che si vuole usare il modello di vista *\Views\[controllerName]\[actionName]* su disco per eseguirne il rendering.

In questo modo è possibile eliminare un po' il codice del controller ed evitare di duplicare il nome due volte nel codice:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Il codice precedente è tutto ciò che serve per implementare un'esperienza di presentazione/dettagli per il sito.

#### <a name="next-step"></a>Passo successivo

A questo punto è stata creata un'esperienza di esplorazione della cena.

A questo punto è possibile abilitare il supporto per la modifica dei form di dati CRUD (creazione, lettura, aggiornamento, eliminazione).

> [!div class="step-by-step"]
> [Precedente](build-a-model-with-business-rule-validations.md)
> [Successivo](provide-crud-create-read-update-delete-data-form-entry-support.md)
