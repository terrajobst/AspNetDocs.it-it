---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Fornire supporto per l'immissione dei dati CRUD (creazione, lettura, aggiornamento, eliminazione) | Microsoft Docs
author: microsoft
description: Il passaggio 5 Mostra come usare ulteriormente la classe DinnersController tramite l'abilitazione del supporto per la modifica, la creazione e l'eliminazione di cene.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: b3123af9a1477bc496a0d229d628510fc202b6d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580556"
---
# <a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Fornire il supporto per operazioni di creazione, lettura, aggiornamento ed eliminazione sulle voci di moduli di dati

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 5 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.
> 
> Il passaggio 5 Mostra come usare ulteriormente la classe DinnersController tramite l'abilitazione del supporto per la modifica, la creazione e l'eliminazione di cene.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner passaggio 5: creare, aggiornare ed eliminare scenari di modulo

Abbiamo introdotto i controller e le visualizzazioni e illustrato come usarli per implementare un'esperienza di elenco/dettagli per le cene sul sito. Il passaggio successivo consisterà nell'eseguire ulteriormente la classe DinnersController e abilitare il supporto per la modifica, la creazione e l'eliminazione di cene.

### <a name="urls-handled-by-dinnerscontroller"></a>URL gestiti da DinnersController

In precedenza sono stati aggiunti metodi di azione a DinnersController che implementavano il supporto per due URL: */dinners* e */dinners/details/[ID]* .

| **URL** | **VERBO** | **Scopo** |
| --- | --- | --- |
| *Cene* | GET | Visualizza un elenco HTML di cene imminenti. |
| */Dinners/Details/[ID]* | GET | Visualizza i dettagli relativi a una cena specifica. |

Verranno ora aggiunti i metodi di azione per implementare tre URL aggiuntivi: */dinners/Edit/[ID]* , */dinners/create*e */dinners/delete/[ID]* . Questi URL consentiranno il supporto per la modifica di cene esistenti, la creazione di nuove cene e l'eliminazione di cene.

Con questi nuovi URL, sono supportate le interazioni HTTP GET e HTTP POST-verbo. Le richieste HTTP GET a questi URL visualizzeranno la visualizzazione HTML iniziale dei dati (un form popolato con i dati della cena nel caso di "Edit", un modulo vuoto nel caso di "create" e una schermata di conferma dell'eliminazione nel caso di "Delete"). Le richieste HTTP POST a questi URL salveranno/aggiorneranno/elimineranno i dati di Dinner in DinnerRepository (e da qui nel database).

| **URL** | **VERBO** | **Scopo** |
| --- | --- | --- |
| */Dinners/Edit/[ID]* | GET | Visualizza un modulo HTML modificabile popolato con i dati di Dinner. |
| INSERISCI | Salvare le modifiche al modulo per una particolare cena nel database. |
| */Dinners/Create* | GET | Visualizza un modulo HTML vuoto che consente agli utenti di definire nuove cene. |
| INSERISCI | Creare un nuovo Dinner e salvarlo nel database. |
| */Dinners/Delete/[ID]* | GET | Visualizza la schermata di conferma dell'eliminazione. |
| INSERISCI | Elimina la cena specificata dal database. |

### <a name="edit-support"></a>Modifica supporto

Si inizia con l'implementazione dello scenario "Edit".

#### <a name="the-http-get-edit-action-method"></a>Metodo di azione di modifica HTTP-GET

Si inizierà implementando il comportamento HTTP "GET" del metodo di azione di modifica. Questo metodo verrà richiamato quando viene richiesto l'URL */dinners/Edit/[ID]* . L'implementazione sarà simile alla seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Il codice precedente USA DinnerRepository per recuperare un oggetto dinner. Viene quindi eseguito il rendering di un modello di visualizzazione utilizzando l'oggetto dinner. Poiché non è stato passato in modo esplicito un nome di modello al metodo helper *View ()* , verrà usato il percorso predefinito basato sulla convenzione per risolvere il modello di vista:/views/dinners/Edit.aspx.

A questo punto, creare questo modello di vista. Questa operazione verrà eseguita facendo clic con il pulsante destro del mouse all'interno del metodo Edit e scegliendo il comando del menu di scelta rapida "Aggiungi visualizzazione":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Nella finestra di dialogo "Aggiungi visualizzazione" si indicherà che è stato passato un oggetto Dinner al modello di visualizzazione come modello e si sceglie di eseguire l'impalcatura automatica di un modello "Edit":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Quando si fa clic sul pulsante "Aggiungi", in Visual Studio viene aggiunto un nuovo file di modello di visualizzazione "Edit. aspx" nella directory "\Views\Dinners". Verrà inoltre aperto il nuovo modello di visualizzazione "Edit. aspx" all'interno dell'editor di codice, popolato con un'implementazione iniziale di un patibolo "Edit", come indicato di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Apportare alcune modifiche all'impalcatura di "modifica" predefinita generata e aggiornare il modello di visualizzazione di modifica in modo da includere il contenuto seguente (che rimuove alcune delle proprietà che non si desidera esporre):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Quando si esegue l'applicazione e si richiede l'URL *"/dinners/Edit/1"* , viene visualizzata la pagina seguente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Il markup HTML generato dalla visualizzazione è simile al seguente. È HTML standard, con un modulo &lt;&gt; elemento che esegue un POST HTTP all'URL */dinners/Edit/1* quando viene effettuato il push del pulsante "salva" &lt;input type = "Submit"/&gt;. Un elemento HTML &lt;input type = "Text"/&gt; è stato restituito per ogni proprietà modificabile:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Metodi helper HTML. BeginForm () e HTML. TextBox ()

Il modello di visualizzazione "Edit. aspx" usa diversi metodi "helper HTML": HTML. ValidationSummary (), HTML. BeginForm (), HTML. TextBox () e HTML. ValidationMessage (). Oltre a generare il markup HTML per Microsoft, questi metodi helper forniscono il supporto predefinito per la gestione degli errori e la convalida.

##### <a name="htmlbeginform-helper-method"></a>Metodo helper HTML. BeginForm ()

Il metodo helper HTML. BeginForm () genera l'output del modulo HTML &lt;&gt; elemento nel markup. Nel modello di visualizzazione Edit. aspx si noterà che si sta applicando un' C# istruzione "using" quando si usa questo metodo. La parentesi graffa aperta indica l'inizio del &lt;form&gt; contenuto e la parentesi graffa di chiusura indica la fine del &lt;/form&gt; elemento:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

In alternativa, se si trova l'approccio dell'istruzione "using" non naturale per uno scenario di questo tipo, è possibile usare una combinazione HTML. BeginForm () e HTML. EndForm () (che esegue la stessa operazione):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Se si chiama HTML. BeginForm () senza parametri, viene restituito un elemento form che esegue un POST HTTP nell'URL della richiesta corrente. Per questo motivo la visualizzazione di modifica genera un' *&lt;form action = "/dinners/Edit/1" Method = "post"&gt;* elemento. In alternativa, è possibile passare parametri espliciti a HTML. BeginForm () se si desidera pubblicare un URL diverso.

##### <a name="htmltextbox-helper-method"></a>Metodo helper HTML. TextBox ()

La vista edit. aspx usa il metodo helper HTML. TextBox () per restituire &lt;input type = "Text"/&gt; Elements:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Il metodo HTML. TextBox () precedente accetta un solo parametro, che viene usato per specificare sia gli attributi ID/Name dell'elemento &lt;input type = "Text"/&gt; per l'output, sia la proprietà Model da cui popolare il valore TextBox. Ad esempio, l'oggetto Dinner passato alla visualizzazione di modifica aveva un valore della proprietà "title" di ".NET Futures", quindi la chiamata al metodo HTML. TextBox ("title") ha restituito l'output: *&lt;input ID = "title" Name = "title" Type = "Text" value = ". NET Futures"/&gt;* .

In alternativa, è possibile usare il primo parametro HTML. TextBox () per specificare l'ID o il nome dell'elemento e quindi passare in modo esplicito il valore da usare come secondo parametro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Spesso è necessario eseguire la formattazione personalizzata del valore restituito. Il metodo statico String. Format () incorporato in .NET è utile per questi scenari. Il modello di vista edit. aspx usa questa operazione per formattare il valore di elemento eventdate (che è di tipo DateTime) in modo che non mostri i secondi per l'ora:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Un terzo parametro di HTML. TextBox () può essere utilizzato facoltativamente per restituire altri attributi HTML. Il frammento di codice seguente illustra come eseguire il rendering di un attributo size = "30" e di un attributo class = "myCssClass" nell'elemento &lt;input type = "Text"/&gt;. Si noti come il nome dell'attributo della classe venga sottoposta a escape usando una "@" character because "class" è una C#parola chiave riservata in:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementazione del metodo di azione HTTP-POST-modifica

A questo punto è stata implementata la versione HTTP-GET del metodo di azione di modifica. Quando un utente richiede l'URL */dinners/Edit/1* , riceve una pagina HTML come la seguente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Se si preme il pulsante "Salva", un form viene pubblicato nell'URL */dinners/Edit/1* e i valori HTML &lt;input&gt; form vengono inviati utilizzando il verbo HTTP post. A questo punto, è possibile implementare il comportamento HTTP POST del metodo di azione di modifica, che gestirà il salvataggio della cena.

Si inizierà con l'aggiunta di un metodo di azione "Edit" di overload al DinnersController con un attributo "AcceptVerbs" che indica che gestisce gli scenari HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Quando l'attributo [AcceptVerbs] viene applicato ai metodi di azione in overload, ASP.NET MVC gestisce automaticamente le richieste di invio al metodo di azione appropriato, a seconda del verbo HTTP in ingresso. Le richieste HTTP POST agli URL */dinners/Edit/[ID]* verranno indirizzate al metodo di modifica precedente, mentre tutte le altre richieste di verbi HTTP agli URL */dinners/Edit/[ID]* verranno indirizzate al primo metodo Edit implementato (che non ha un attributo `[AcceptVerbs]`).

| **Argomento laterale: perché distinguere i verbi HTTP?** |
| --- |
| È possibile chiedersi: perché si usa un singolo URL e si distingue il comportamento tramite il verbo HTTP? Perché non sono disponibili solo due URL distinti per gestire il caricamento e il salvataggio delle modifiche di modifica? Ad esempio:/Dinners/Edit/[ID] per visualizzare il modulo iniziale e/Dinners/Save/[ID] per gestire il post del modulo per salvarlo? Il lato negativo della pubblicazione di due URL distinti è che nei casi in cui viene pubblicato in/Dinners/Save/2 e quindi è necessario rivisualizzare il form HTML a causa di un errore di input, l'utente finale deve avere l'URL/Dinners/Save/2 nella barra degli indirizzi del browser (poiché questo è l'URL in cui è stato inserito il modulo). Se l'utente finale inserisce in segnalibro questa pagina rivisualizzata nell'elenco Preferiti del browser o copia/incolla l'URL e lo invia tramite posta elettronica a un amico, il salvataggio di un URL che non funzionerà in futuro (poiché tale URL dipende da valori post). Esponendo un singolo URL (ad esempio:/Dinners/Edit/[ID]) e differenziando l'elaborazione in base al verbo HTTP, gli utenti finali possono aggiungere un segnalibro alla pagina di modifica e/o inviare l'URL ad altri. |

#### <a name="retrieving-form-post-values"></a>Recupero dei valori post del form

Esistono diversi modi per accedere ai parametri del modulo inviati nel metodo "Edit" HTTP POST. Un approccio semplice consiste nell'usare semplicemente la proprietà Request della classe di base controller per accedere alla raccolta form e recuperare direttamente i valori inseriti:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Tuttavia, l'approccio precedente è un po' dettagliato, soprattutto quando si aggiunge la logica di gestione degli errori.

Un approccio migliore per questo scenario consiste nell'utilizzare il metodo helper *UpdateModel ()* incorporato sulla classe di base controller. Supporta l'aggiornamento delle proprietà di un oggetto che viene passato utilizzando i parametri del form in ingresso. Usa la reflection per determinare i nomi delle proprietà nell'oggetto, quindi converte automaticamente e assegna i valori in base ai valori di input inviati dal client.

È possibile usare il metodo UpdateModel () per semplificare l'azione HTTP-POST Edit usando questo codice:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

È ora possibile visitare l'URL di */dinners/Edit/1* e modificare il titolo della cena:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Quando si fa clic sul pulsante "Save" (Salva), verrà eseguito un post del modulo per l'azione di modifica e i valori aggiornati verranno salvati in modo permanente nel database. Si verrà quindi reindirizzati all'URL dei dettagli per la cena (in cui verranno visualizzati i valori appena salvati):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Gestione degli errori di modifica

L'implementazione HTTP-POST corrente funziona correttamente, tranne quando si verificano errori.

Quando un utente commette un errore durante la modifica di un modulo, è necessario assicurarsi che il modulo venga visualizzato nuovamente con un messaggio di errore informativo che li guida per risolverlo. Sono inclusi i casi in cui un utente finale invia un input errato (ad esempio, una stringa di data in formato non valido), nonché i casi in cui il formato di input è valido, ma è presente una violazione della regola business. Quando si verificano errori, il form deve conservare i dati di input immessi originariamente dall'utente in modo da non dover ricompilare manualmente le modifiche. Questo processo deve essere ripetuto il numero di volte necessario finché il modulo non viene completato correttamente.

ASP.NET MVC include alcune interessanti funzionalità predefinite che semplificano la gestione degli errori e la rivisualizzazione dei moduli. Per visualizzare queste funzionalità in azione, è necessario aggiornare il metodo di azione di modifica con il codice seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Il codice precedente è simile all'implementazione precedente, ad eccezione del fatto che viene eseguito il wrapping di un blocco di gestione degli errori try/catch intorno al lavoro. Se si verifica un'eccezione quando si chiama UpdateModel () o quando si tenta di salvare DinnerRepository (che genererà un'eccezione se l'oggetto Dinner che si sta tentando di salvare non è valido a causa di una violazione della regola all'interno del modello), il blocco di gestione degli errori catch verrà eseguire. Al suo interno viene effettuato il ciclo su qualsiasi violazione di regola presente nell'oggetto Dinner e la si aggiunge a un oggetto ModelState, che verrà illustrato a breve. Viene quindi visualizzata nuovamente la visualizzazione.

Per verificarne il funzionamento, è necessario eseguire di nuovo l'applicazione, modificare una cena e modificarla in modo che abbia un titolo vuoto, un elemento eventdate di "FASULLo" e usare un numero di telefono del Regno Unito con un valore di paese di Stati Uniti. Quando si preme il pulsante "Save" (Salva) il metodo HTTP POST Edit non sarà in grado di salvare la cena (a causa di errori) e verrà visualizzato nuovamente il modulo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

L'applicazione ha un'esperienza di errore decente. Gli elementi di testo con l'input non valido vengono evidenziati in rosso e i messaggi di errore di convalida vengono visualizzati dall'utente finale. Il modulo conserva anche i dati di input immessi originariamente dall'utente, in modo che non debbano riempire nulla.

In che modo è possibile chiedersi se questa situazione si verifica? In che modo le caselle di testo title, elemento eventdate e ContactPhone vengono evidenziate in rosso e sono in grado di restituire i valori utente immessi originariamente? E in che modo i messaggi di errore vengono visualizzati nell'elenco in alto? L'aspetto positivo è che questa situazione non si è verificata da Magic, bensì dal fatto che sono state usate alcune delle funzionalità di MVC ASP.NET predefinite che semplificano la convalida dell'input e gli scenari di gestione degli errori.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Informazioni su ModelState e sui metodi helper HTML di convalida

Le classi controller hanno una raccolta di proprietà "ModelState" che fornisce un modo per indicare che sono presenti errori con un oggetto modello passato a una visualizzazione. Le voci di errore all'interno della raccolta ModelState identificano il nome della proprietà del modello con il problema (ad esempio, "title", "elemento eventdate" o "ContactPhone") e consentono di specificare un messaggio di errore facile da usare, ad esempio: "title è obbligatorio".

Il metodo helper *UpdateModel ()* popola automaticamente la raccolta ModelState quando si verificano errori durante il tentativo di assegnare i valori del modulo alle proprietà nell'oggetto modello. Ad esempio, la proprietà elemento eventdate dell'oggetto Dinner è di tipo DateTime. Quando il metodo UpdateModel () non è riuscito a assegnare il valore stringa "FASULLo" nello scenario precedente, il metodo UpdateModel () ha aggiunto una voce alla raccolta ModelState che indica che si è verificato un errore di assegnazione con tale proprietà.

Gli sviluppatori possono anche scrivere codice per aggiungere in modo esplicito le voci di errore nella raccolta ModelState, come in seguito nel blocco di gestione degli errori "catch", che popola la raccolta ModelState con le voci basate sulle violazioni delle regole attive nel Oggetto Dinner:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integrazione dell'helper HTML con ModelState

Metodi helper HTML, ad esempio HTML. TextBox (): controllare la raccolta ModelState durante il rendering dell'output. Se esiste un errore per l'elemento, eseguono il rendering del valore immesso dall'utente e di una classe di errore CSS.

Ad esempio, nella vista "Edit" viene usato il metodo helper HTML. TextBox () per eseguire il rendering del elemento eventdate dell'oggetto Dinner:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Quando è stato eseguito il rendering della vista nello scenario di errore, il metodo HTML. TextBox () ha controllato la raccolta ModelState per verificare se sono stati associati errori alla proprietà "elemento eventdate" dell'oggetto dinner. Quando è stato determinato che si è verificato un errore durante il rendering dell'input dell'utente inviato ("FASULLo") come valore, è stata aggiunta una classe di errore CSS al &lt;input type = "TextBox"/&gt; markup generato:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

È possibile personalizzare l'aspetto della classe di errore CSS per l'aspetto desiderato. La classe di errore CSS predefinita, "Input-Validation-Error", è definita nel foglio di stile *\content\site.CSS* e ha un aspetto simile al seguente:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Questa regola CSS è la causa dell'evidenziazione degli elementi di input non validi come riportato di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Metodo helper HTML. ValidationMessage ()

Il metodo helper HTML. ValidationMessage () può essere utilizzato per restituire il messaggio di errore ModelState associato a una particolare proprietà del modello:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Il codice riportato sopra restituisce: *&lt;span class = "Field-Validation-Error"&gt; il valore ' false ' non è valido&lt;/span&gt;*

Il metodo helper HTML. ValidationMessage () supporta anche un secondo parametro che consente agli sviluppatori di eseguire l'override del messaggio di testo di errore visualizzato:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Il codice riportato sopra restituisce: *&lt;span class = "Field-Validation-Error"&gt;\*&lt;/span&gt;* anziché il testo di errore predefinito quando è presente un errore per la proprietà elemento eventdate.

##### <a name="htmlvalidationsummary-helper-method"></a>Metodo helper HTML. ValidationSummary ()

Il metodo helper HTML. ValidationSummary () può essere usato per eseguire il rendering di un messaggio di errore di riepilogo, accompagnato da un &lt;UL&gt;&lt;li/&gt;&lt;/UL&gt; elenco di tutti i messaggi di errore dettagliati nella raccolta ModelState:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Il metodo helper HTML. ValidationSummary () accetta un parametro di stringa facoltativo, che definisce un messaggio di errore di riepilogo da visualizzare sopra l'elenco degli errori dettagliati:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Facoltativamente, è possibile utilizzare i componenti CSS per ignorare l'aspetto dell'elenco errori.

#### <a name="using-a-addruleviolations-helper-method"></a>Uso di un metodo helper AddRuleViolations

L'implementazione iniziale di HTTP-POST Edit ha usato un'istruzione foreach all'interno del blocco catch per eseguire il ciclo sulle violazioni delle regole dell'oggetto Dinner e aggiungerle alla raccolta ModelState del controller:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

È possibile rendere questo codice un po' più pulito aggiungendo una classe "ControllerHelpers" al progetto NerdDinner e implementare un metodo di estensione "AddRuleViolations" al suo interno che aggiunge un metodo helper alla classe ASP.NET MVC ModelStateDictionary. Questo metodo di estensione può incapsulare la logica necessaria per popolare ModelStateDictionary con un elenco di errori RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

È quindi possibile aggiornare il metodo di azione HTTP-POST-modifica per usare questo metodo di estensione per popolare la raccolta ModelState con le violazioni della regola dinner.

#### <a name="complete-edit-action-method-implementations"></a>Completare le implementazioni del metodo di azione di modifica

Il codice riportato di seguito implementa tutta la logica del controller necessaria per lo scenario di modifica:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

L'aspetto interessante della nostra implementazione di modifica è che né la nostra classe controller, né il nostro modello di visualizzazione, debbano sapere nulla sulla convalida specifica o sulle regole business applicate dal nostro modello di Dinner. È possibile aggiungere altre regole al modello in futuro e non è necessario apportare modifiche al codice per il controller o la visualizzazione per poter essere supportate. Questo offre la flessibilità necessaria per sviluppare facilmente i requisiti delle applicazioni in futuro con un minimo di modifiche al codice.

### <a name="create-support"></a>Creazione del supporto

È stata completata l'implementazione del comportamento "Edit" della classe DinnersController. Ora è possibile procedere con l'implementazione del supporto "create" (Crea), che consente agli utenti di aggiungere nuove cene.

#### <a name="the-http-get-create-action-method"></a>Metodo di azione HTTP GET create

Si inizierà implementando il comportamento HTTP "GET" del metodo di azione create. Questo metodo viene chiamato quando un utente visita l'URL */dinners/create* . L'implementazione è simile alla seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Il codice precedente crea un nuovo oggetto Dinner e assegna la relativa proprietà elemento eventdate a una settimana in futuro. Viene quindi eseguito il rendering di una visualizzazione basata sul nuovo oggetto dinner. Poiché non è stato passato esplicitamente un nome al metodo helper *View ()* , verrà usato il percorso predefinito basato sulla convenzione per risolvere il modello di vista:/views/dinners/create.aspx.

A questo punto, creare questo modello di vista. A tale scopo, fare clic con il pulsante destro del mouse all'interno del metodo crea azione e scegliere il comando del menu di scelta rapida "Aggiungi visualizzazione". Nella finestra di dialogo "Aggiungi visualizzazione" si indicherà che si passa un oggetto Dinner al modello di visualizzazione e si sceglie di eseguire l'impalcatura automatica di un modello "create":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio salva una nuova visualizzazione "create. aspx" basata su impalcature nella directory "\Views\Dinners" e la apre all'interno dell'IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Apportare alcune modifiche al file di impalcatura "create" predefinito che è stato generato per noi e modificarlo in modo che sia simile al seguente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

A questo punto, quando si esegue l'applicazione e si accede all'URL *"/dinners/create"* all'interno del browser, verrà eseguito il rendering dell'interfaccia utente come riportato di seguito dall'implementazione dell'azione di creazione:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementazione del metodo di azione HTTP-POST-creazione

È stata implementata la versione HTTP-GET del metodo di creazione dell'azione. Quando un utente fa clic sul pulsante "Save" (Salva), esegue un post di form nell'URL */dinners/create* e invia i valori di input &lt;HTML&gt; form usando il verbo HTTP post.

A questo punto, è possibile implementare il comportamento HTTP POST del metodo di azione create. Si inizierà aggiungendo un metodo di azione "create" di overload al DinnersController con un attributo "AcceptVerbs" che indica che gestisce gli scenari HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Esistono diversi modi per accedere ai parametri del modulo inviati all'interno del metodo "create" abilitato per HTTP-POST.

Un approccio consiste nel creare un nuovo oggetto Dinner e quindi usare il metodo helper *UpdateModel ()* , come è stato fatto con l'azione Edit, per popolarlo con i valori del modulo inviati. È quindi possibile aggiungerlo alla DinnerRepository, salvarlo in modo permanente nel database e reindirizzare l'utente all'azione dettagli per mostrare la cena appena creata usando il codice seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

In alternativa, è possibile usare un approccio in cui il metodo di azione Create () accetta un oggetto Dinner come parametro del metodo. ASP.NET MVC creerà automaticamente un'istanza di un nuovo oggetto Dinner, ne popola le proprietà usando gli input del form e lo passa al metodo di azione:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Il metodo di azione sopra riportato verifica che l'oggetto Dinner sia stato popolato correttamente con i valori post del form controllando la proprietà ModelState. IsValid. Viene restituito false se sono presenti problemi di conversione di input (ad esempio, una stringa di "FASULLo" per la proprietà elemento eventdate) e in caso di problemi il metodo di azione rivisualizzerà il modulo.

Se i valori di input sono validi, il metodo di azione tenta di aggiungere e salvare la nuova cena in DinnerRepository. Esegue il wrapping di questa operazione all'interno di un blocco try/catch e visualizza nuovamente il form in caso di violazioni delle regole business (che comporterebbe il metodo dinnerRepository. Save () per generare un'eccezione).

Per visualizzare questo comportamento di gestione degli errori in azione, è possibile richiedere l'URL */dinners/create* e compilare i dettagli relativi a una nuova cena. L'input o i valori non corretti provocheranno la rivisualizzazione del modulo di creazione con gli errori evidenziati come riportato di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Si noti che il modulo di creazione rispetta esattamente la stessa convalida e le stesse regole di business del modulo di modifica. Ciò è dovuto al fatto che le regole business e di convalida sono state definite nel modello e non sono state incorporate all'interno dell'interfaccia utente o del controller dell'applicazione. Ciò significa che in un secondo momento sarà possibile modificare/evolvere le regole di business o di convalida in un'unica posizione e applicarle all'interno dell'applicazione. Non sarà necessario modificare il codice all'interno dei metodi di azione di modifica o creazione per rispettare automaticamente eventuali nuove regole o modifiche a quelle esistenti.

Quando si correggono i valori di input e si fa nuovamente clic sul pulsante "Save" (Salva), l'aggiunta di DinnerRepository avrà esito positivo e verrà aggiunta una nuova cena al database. Si verrà quindi reindirizzati all'URL */dinners/details/[ID]* , in cui verranno presentati i dettagli relativi alla cena appena creata:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Elimina supporto

A questo punto, aggiungere il supporto "Delete" al DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Metodo di azione HTTP-GET Delete

Si inizierà implementando il comportamento HTTP GET del metodo di azione Delete. Questo metodo viene chiamato quando un utente visita l'URL */dinners/delete/[ID]* . Di seguito è riportata l'implementazione di:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Il metodo di azione tenta di recuperare la cena da eliminare. Se la cena esiste, viene eseguito il rendering di una visualizzazione basata sull'oggetto dinner. Se l'oggetto non esiste o è già stato eliminato, restituisce una visualizzazione che esegue il rendering del modello di visualizzazione "NotFound" creato in precedenza per il metodo di azione "Details".

È possibile creare il modello di visualizzazione "Elimina" facendo clic con il pulsante destro del mouse all'interno del metodo di azione Delete e selezionando il comando del menu di scelta rapida "Aggiungi visualizzazione". Nella finestra di dialogo "Aggiungi visualizzazione" si indicherà che è stato passato un oggetto Dinner al modello di visualizzazione come modello e si sceglie di creare un modello vuoto:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Quando si fa clic sul pulsante "Add" (Aggiungi), in Visual Studio viene aggiunto un nuovo file di modello di visualizzazione "Delete. aspx" nella directory "\Views\Dinners". Aggiungeremo codice HTML e codice al modello per implementare una schermata di conferma dell'eliminazione come riportato di seguito:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Nel codice precedente viene visualizzato il titolo della cena da eliminare e viene restituito un &lt;form&gt; elemento che esegue un POST sull'URL/Dinners/Delete/[ID] se l'utente finale fa clic sul pulsante "Elimina" al suo interno.

Quando si esegue l'applicazione e si accede all'URL *"/dinners/delete/[ID]"* per un oggetto Dinner valido, viene eseguito il rendering dell'interfaccia utente come riportato di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Argomento laterale: perché si sta eseguendo un POST?** |
| --- |
| Si potrebbe chiedere: perché è stato possibile creare un modulo &lt;&gt; all'interno della schermata di conferma dell'eliminazione? Perché non usare semplicemente un collegamento ipertestuale standard per collegarsi a un metodo di azione che esegue l'operazione di eliminazione effettiva? Il motivo è che è opportuno prestare attenzione a proteggersi da web crawler e dai motori di ricerca per individuare gli URL e causare inavvertitamente l'eliminazione dei dati quando seguono i collegamenti. Gli URL basati su HTTP-GET sono considerati "sicuri" per l'accesso o la ricerca per indicizzazione e dovrebbero non seguire quelli HTTP-POST. Una regola efficace consiste nel garantire che le operazioni di modifica dei dati o distruttive vengano sempre eseguite dietro richieste HTTP-POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementazione del metodo di azione HTTP-POST Delete

È ora disponibile la versione HTTP GET del metodo di azione Delete implementato che visualizza una schermata di conferma dell'eliminazione. Quando un utente finale fa clic sul pulsante "Elimina", eseguirà un post del form nell'URL */dinners/Dinner/[ID]* .

A questo punto, è possibile implementare il comportamento HTTP "POST" del metodo di azione Delete usando il codice seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La versione HTTP-POST del metodo di azione DELETE tenta di recuperare l'oggetto Dinner da eliminare. Se non riesce a trovarla (perché è già stata eliminata), esegue il rendering del modello "NotFound". Se trova la cena, la Elimina dal DinnerRepository. Esegue quindi il rendering di un modello "eliminato".

Per implementare il modello "eliminato", fare clic con il pulsante destro del mouse sul metodo di azione e scegliere il menu di scelta rapida "Aggiungi visualizzazione". Il nome della vista è "Deleted" e deve essere un modello vuoto (che non accetta un oggetto modello fortemente tipizzato). Si aggiungerà quindi del contenuto HTML:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

A questo punto, quando si esegue l'applicazione e si accede all'URL *"/dinners/delete/[ID]"* per un oggetto Dinner valido, verrà eseguito il rendering della schermata di conferma dell'eliminazione della cena come riportato di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Quando si fa clic sul pulsante "Elimina", viene eseguito un HTTP-POST nell'URL */dinners/delete/[ID]* , che eliminerà la cena dal database e visualizzerà il modello di visualizzazione "eliminato":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Sicurezza dell'associazione di modelli

Sono stati discussi due modi diversi per usare le funzionalità di associazione di modelli predefinite di ASP.NET MVC. Il primo usa il metodo UpdateModel () per aggiornare le proprietà di un oggetto modello esistente e il secondo usando il supporto di ASP.NET MVC per passare gli oggetti modello in come parametri del metodo di azione. Entrambe queste tecniche sono molto potenti ed estremamente utili.

Questa potenza offre anche responsabilità it. Quando si accettano gli input degli utenti, è importante avere sempre una paranoia per la sicurezza, anche se è vero quando si associano oggetti per formare input. È necessario prestare attenzione a codificare sempre in HTML tutti i valori immessi dall'utente per evitare attacchi intrusivi in HTML e JavaScript e prestare attenzione agli attacchi intrusivi nel codice SQL (Nota: si sta usando LINQ to SQL per l'applicazione, che codifica automaticamente i parametri per evitare questi tipi di attacchi. Si consiglia di non basarsi mai sulla convalida lato client e di utilizzare sempre la convalida sul lato server per impedire agli hacker che tentano di inviare valori fasulli.

Un altro elemento di sicurezza per assicurarsi di considerare quando si usano le funzionalità di associazione di ASP.NET MVC è l'ambito degli oggetti da associare. In particolare, è necessario assicurarsi di aver compreso le implicazioni di sicurezza delle proprietà che si desidera associare e assicurarsi di consentire l'aggiornamento solo delle proprietà che dovrebbero effettivamente essere aggiornabili da un utente finale.

Per impostazione predefinita, il metodo UpdateModel () tenterà di aggiornare tutte le proprietà dell'oggetto modello che corrispondono ai valori dei parametri del form in ingresso. Analogamente, per impostazione predefinita, anche gli oggetti passati come parametri del metodo di azione possono avere tutte le relative proprietà impostate tramite i parametri del modulo.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Blocco dell'associazione in base all'utilizzo

È possibile bloccare i criteri di associazione per ogni singolo utilizzo fornendo un "elenco di inclusione" esplicito di proprietà che possono essere aggiornate. Questa operazione può essere eseguita passando un parametro di matrice di stringhe aggiuntivo al metodo UpdateModel () come riportato di seguito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Gli oggetti passati come parametri del metodo di azione supportano anche un attributo [bind] che consente di specificare un "elenco di inclusione" delle proprietà consentite come riportato di seguito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Blocco del binding in base al tipo

È anche possibile bloccare le regole di binding in base al tipo. In questo modo è possibile specificare le regole di associazione una volta e quindi applicarle in tutti gli scenari, inclusi gli scenari di parametri UpdateModel e di metodo di azione, in tutti i controller e i metodi di azione.

Per personalizzare le regole di associazione per tipo, è possibile aggiungere un attributo [bind] a un tipo o registrarlo all'interno del file Global. asax dell'applicazione (utile per gli scenari in cui non si è proprietari del tipo). È quindi possibile usare le proprietà include ed exclude dell'attributo bind per controllare quali proprietà sono associabili per la classe o l'interfaccia particolare.

Questa tecnica verrà usata per la classe Dinner nell'applicazione NerdDinner e verrà aggiunto un attributo [bind] che limita l'elenco di proprietà associabili a quanto segue:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Si noti che non è possibile modificare la raccolta RSVPs tramite binding, né consentire l'impostazione delle proprietà DinnerID o HostedBy tramite binding. Per motivi di sicurezza, verranno invece modificate solo queste proprietà specifiche utilizzando codice esplicito all'interno dei metodi di azione.

### <a name="crud-wrap-up"></a>Wrapping CRUD

ASP.NET MVC include numerose funzionalità predefinite che consentono di implementare scenari di inserimento dei moduli. Sono state usate diverse funzionalità per fornire il supporto dell'interfaccia utente CRUD nella DinnerRepository.

Viene utilizzato un approccio basato sul modello per implementare l'applicazione. Ciò significa che tutta la logica delle regole business e di convalida è definita all'interno del livello del modello, non all'interno di controller o visualizzazioni. Né la classe controller né i modelli di visualizzazione sono in grado di conoscere le regole business specifiche applicate dalla classe del modello dinner.

Questa operazione manterrà pulita l'architettura dell'applicazione e ne renderà più semplice il test. È possibile aggiungere altre regole di business al livello del modello in futuro e *non è necessario apportare modifiche al codice* per il controller o la visualizzazione in modo che siano supportate. In questo modo ci si offrirà una grande flessibilità per evolversi e modificare l'applicazione in futuro.

Il nostro DinnersController ora Abilita gli elenchi di cene e i dettagli, nonché il supporto per la creazione, la modifica e l'eliminazione. Il codice completo per la classe è disponibile di seguito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Passo successivo

A questo punto è disponibile il supporto CRUD (creazione, lettura, aggiornamento ed eliminazione) di base implementato all'interno della classe DinnersController.

Si esaminerà ora come è possibile usare le classi ViewData e ViewModel per abilitare l'interfaccia utente ancora più ricca nei form.

> [!div class="step-by-step"]
> [Precedente](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Successivo](use-viewdata-and-implement-viewmodel-classes.md)
