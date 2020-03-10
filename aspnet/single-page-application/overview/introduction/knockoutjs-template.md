---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Applicazione a pagina singola: modello knockout | Microsoft Docs'
author: MikeWasson
description: Modello knockout
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 3a551db1caa9636eb7f2e04c287d3ef371263584
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578694"
---
# <a name="single-page-application-knockoutjs-template"></a>Applicazione a pagina singola: modello knockout

di [Mike Wasson](https://github.com/MikeWasson)

> Il modello MVC Knockout fa parte di ASP.NET and Web Tools 2012,2
> 
> [Scarica ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

L'aggiornamento di ASP.NET and Web Tools 2012,2 include un modello di applicazione a pagina singola (SPA) per ASP.NET MVC 4. Questo modello è stato progettato per iniziare rapidamente a creare app Web interattive sul lato client.

"Applicazione a pagina singola" (SPA) è il termine generale per un'applicazione Web che carica una singola pagina HTML e quindi aggiorna la pagina in modo dinamico, anziché caricare nuove pagine. Al termine del caricamento iniziale della pagina, la SPA comunica con il server tramite richieste AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX non è una novità, ma oggi sono disponibili Framework JavaScript che semplificano la creazione e la gestione di un'applicazione SPA sofisticata di grandi dimensioni. Inoltre, HTML 5 e CSS3 semplificano la creazione di interfacce utente avanzate.

Per iniziare, il modello SPA crea un'applicazione "elenco attività" di esempio. In questa esercitazione verrà illustrata una presentazione guidata del modello. Prima di tutto si osserverà l'applicazione to-do list, quindi si analizzeranno i componenti tecnologici che lo rendono funzionante.

## <a name="create-a-new-spa-template-project"></a>Creare un nuovo progetto di modello SPA

Requisiti:

- Visual Studio 2012 o Visual Studio Express 2012 per il Web
- ASP.NET Web Tools 2012,2 Update. È possibile installare l'aggiornamento [qui](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Avviare Visual Studio e selezionare **nuovo progetto** nella pagina iniziale. In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** . In **Visual C#** Selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET MVC 4**. Assegnare un nome al progetto e fare clic su **OK**.

![](knockoutjs-template/_static/image2.png)

Nella creazione guidata **nuovo progetto** selezionare **applicazione a pagina singola**.

![](knockoutjs-template/_static/image3.png)

Premere F5 per compilare ed eseguire l'applicazione. Quando l'applicazione viene eseguita per la prima volta, viene visualizzata una schermata di accesso.

![](knockoutjs-template/_static/image4.png)

Fare clic sul collegamento &quot;iscrizione&quot; e creare un nuovo utente.

![](knockoutjs-template/_static/image5.png)

Dopo aver eseguito l'accesso, l'applicazione crea un elenco TODO predefinito con due elementi. È possibile fare clic su "Aggiungi elenco TODO" per aggiungere un nuovo elenco.

![](knockoutjs-template/_static/image6.png)

Rinominare l'elenco, aggiungere elementi all'elenco e deselezionarli. È anche possibile eliminare elementi o eliminare un intero elenco. Le modifiche vengono salvate automaticamente in modo permanente in un database nel server (in realtà local DB a questo punto, perché l'applicazione viene eseguita localmente).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architettura del modello SPA

Questo diagramma mostra i blocchi predefiniti principali per l'applicazione.

![](knockoutjs-template/_static/image8.png)

Sul lato server, ASP.NET MVC serve il codice HTML e gestisce anche l'autenticazione basata su form.

API Web ASP.NET gestisce tutte le richieste correlate a ToDoLists e ToDoItems, inclusi il recupero, la creazione, l'aggiornamento e l'eliminazione. Il client scambia i dati con l'API Web in formato JSON.

Entity Framework (EF) è il livello O/RM. Viene mediato tra il mondo orientato a oggetti di ASP.NET e il database sottostante. Il database utilizza il database locale, ma è possibile modificarlo nel file Web. config. In genere si usa il database locale per lo sviluppo locale e quindi si esegue la distribuzione in un database SQL nel server, usando la migrazione del Code-First di EF.

Sul lato client, la libreria knockout. js gestisce gli aggiornamenti della pagina dalle richieste AJAX. Knockout USA data binding per sincronizzare la pagina con i dati più recenti. In questo modo, non è necessario scrivere codice che analizza i dati JSON e aggiorna il DOM. Al contrario, si inseriscono gli attributi dichiarativi nel codice HTML che indicano a knockout come presentare i dati.

Un grande vantaggio di questa architettura è la separazione del livello di presentazione dalla logica dell'applicazione. È possibile creare la parte dell'API Web senza conoscere l'aspetto della pagina Web. Sul lato client viene creato un "modello di visualizzazione" per rappresentare i dati e il modello di visualizzazione USA knockout per l'associazione al codice HTML. Questo consente di modificare facilmente il codice HTML senza modificare il modello di visualizzazione. (Verrà esaminato un po' più avanti).

## <a name="models"></a>Modelli

Nel progetto di Visual Studio la cartella Models contiene i modelli utilizzati sul lato server. Sono disponibili anche modelli sul lato client, che verranno riportati.

![](knockoutjs-template/_static/image9.png)

**TodoItem, todo**

Questi sono i modelli di database per Entity Framework Code First. Si noti che questi modelli hanno proprietà che puntano l'uno all'altro. `ToDoList` contiene una raccolta di ToDoItems e ogni `ToDoItem` riporta un riferimento al relativo oggetto padre. Queste proprietà sono denominate proprietà di navigazione e rappresentano la relazione uno-a-molti un elenco attività e i relativi elementi attività.

La classe `ToDoItem` usa anche l'attributo **[ForeignKey]** per specificare che `ToDoListId` è una chiave esterna nella tabella `ToDoList`. Ciò indica a EF di aggiungere un vincolo FOREIGN KEY al database.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Queste classi definiscono i dati che verranno inviati al client. "DTO" è l'acronimo di "Data Transfer Object". DTO definisce il modo in cui le entità verranno serializzate in JSON. In generale, esistono diversi motivi per usare dto:

- Per controllare quali proprietà vengono serializzate. DTO può contenere un subset delle proprietà del modello di dominio. Questa operazione può essere eseguita per motivi di sicurezza (per nascondere i dati sensibili) o semplicemente per ridurre la quantità di dati inviati.
- Per modificare la forma dei dati, ad esempio per rendere flat una struttura di dati più complessa.
- Per evitare la logica di business DTO (separazione dei problemi).
- Se per qualche motivo non è possibile serializzare i modelli di dominio. Ad esempio, i riferimenti circolari possono causare problemi durante la serializzazione di un oggetto. sono disponibili modi per gestire questo problema nell'API Web (vedere [gestione di riferimenti a oggetti circolari](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)). Tuttavia, l'uso di un DTO semplicemente evita il problema.

Nel modello SPA, dto contiene gli stessi dati dei modelli di dominio. Tuttavia, sono ancora utili perché evitano riferimenti circolari dalle proprietà di navigazione e dimostrano il modello DTO generale.

**AccountModels.cs**

Questo file contiene i modelli per l'appartenenza al sito. La classe `UserProfile` definisce lo schema per i profili utente nel database di appartenenze. (In questo caso, le uniche informazioni sono l'ID utente e il nome utente). Le altre classi del modello in questo file vengono usate per creare i moduli di registrazione e di accesso dell'utente.

## <a name="entity-framework"></a>Entity Framework

Il modello SPA USA Code First EF. In Code First sviluppo, i modelli vengono definiti prima nel codice e quindi EF utilizza il modello per creare il database. È anche possibile usare EF con un database esistente ([database First](https://msdn.microsoft.com/data/jj206878.aspx)).

La classe `TodoItemContext` nella cartella Models deriva da **DbContext**. Questa classe fornisce il "glue" tra i modelli e EF. Il `TodoItemContext` include una raccolta di `ToDoItem` e una raccolta di `TodoList`. Per eseguire una query sul database, è sufficiente scrivere una query LINQ su queste raccolte. Ecco ad esempio come è possibile selezionare tutti gli elenchi attività per l'utente "Alice":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

È inoltre possibile aggiungere nuovi elementi alla raccolta, aggiornare elementi o eliminare elementi dalla raccolta e salvare in modo permanente le modifiche apportate al database.

## <a name="aspnet-web-api-controllers"></a>Controller di API Web ASP.NET

In API Web ASP.NET i controller sono oggetti che gestiscono le richieste HTTP. Come indicato in precedenza, il modello di applicazione a singola pagina usa l'API Web per abilitare le operazioni CRUD nelle istanze `ToDoList` e `ToDoItem`. I controller si trovano nella cartella controller della soluzione.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: gestisce le richieste HTTP per gli elementi attività
- `TodoListController`: gestisce le richieste HTTP per gli elenchi di attività.

Questi nomi sono significativi, perché l'API Web corrisponde al percorso URI al nome del controller. Per informazioni su come l'API Web instrada le richieste HTTP ai controller, vedere [routing in API Web ASP.NET](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

Esaminiamo la classe `ToDoListController`. Contiene un singolo membro dati:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

Il `TodoItemContext` viene usato per comunicare con EF, come descritto in precedenza. I metodi sul controller implementano le operazioni CRUD. API Web esegue il mapping delle richieste HTTP dal client ai metodi del controller, come indicato di seguito:

| Richiesta HTTP | Controller (metodo) | Description |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Ottiene una raccolta di elenchi di attività. |
| Ottieni*ID* /API/todo/ | `GetTodoList` | Ottiene un elenco attività in base all'ID |
| Inserisci*ID* /API/todo/ | `PutTodoList` | Aggiorna un elenco attività. |
| POST /api/todo | `PostTodoList` | Crea un nuovo elenco attività. |
| Elimina*ID* /API/todo/ | `DeleteTodoList` | Elimina un elenco TODO. |

Si noti che gli URI per alcune operazioni contengono segnaposto per il valore ID. Ad esempio, per eliminare un elenco con ID 42, l'URI viene `/api/todo/42`.

Per altre informazioni sull'uso dell'API Web per le operazioni CRUD, vedere [creazione di un'API Web che supporta operazioni CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Il codice per questo controller è piuttosto semplice. Ecco alcuni punti interessanti:

- Il metodo `GetTodoLists` usa una query LINQ per filtrare i risultati in base all'ID dell'utente che ha eseguito l'accesso. In questo modo, un utente vedrà solo i dati che vi appartengono. Si noti inoltre che un'istruzione SELECT viene utilizzata per convertire le istanze di `ToDoList` in `TodoListDto` istanze.
- I metodi PUT e POST controllano lo stato del modello prima di modificare il database. Se **ModelState. IsValid** è false, questi metodi restituiscono http 400, richiesta non valida. Altre informazioni sulla convalida del modello in API Web sono valide per la [convalida del modello](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- La classe controller viene inoltre decorata con l'attributo **[autorizzate]** . Questo attributo controlla se la richiesta HTTP è autenticata. Se la richiesta non è autenticata, il client riceve HTTP 401, non autorizzato. Per altre informazioni sull'autenticazione [, vedere autenticazione e autorizzazione in API Web ASP.NET](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

La classe `TodoController` è molto simile a `TodoListController`. La differenza principale consiste nel fatto che non definisce metodi GET, perché il client otterrà gli elementi attività insieme a ogni elenco attività.

## <a name="mvc-controllers-and-views"></a>Controller e visualizzazioni MVC

I controller MVC si trovano anche nella cartella controller della soluzione. `HomeController` esegue il rendering del codice HTML principale per l'applicazione. La visualizzazione per il controller Home è definita in views/Home/index. cshtml. La visualizzazione Home esegue il rendering di contenuto diverso a seconda che l'utente sia connesso:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Quando gli utenti accedono, visualizzano l'interfaccia utente principale. In caso contrario, viene visualizzato il pannello di accesso. Si noti che questo rendering condizionale si verifica sul lato server. Non provare mai a nascondere il contenuto sensibile sul lato&#8212;client, un elemento inviato in una risposta HTTP è visibile a un utente che sta controllando i messaggi HTTP non elaborati.

## <a name="client-side-javascript-and-knockoutjs"></a>JavaScript sul lato client e Knockout. js

A questo punto è possibile trasformare il lato server dell'applicazione nel client. Il modello SPA usa una combinazione di jQuery ed knockout. js per creare un'interfaccia utente interattiva e uniforme. Knockout. js è una libreria JavaScript che semplifica l'associazione di HTML ai dati. Knockout. js usa un modello denominato "Model-View-ViewModel".

- Il modello è i dati di dominio (elenchi ToDo e elementi ToDo).
- La visualizzazione è il documento HTML.
- Il modello di visualizzazione è un oggetto JavaScript che include i dati del modello. Il modello di visualizzazione è un'astrazione del codice dell'interfaccia utente. Non conosce la rappresentazione HTML. Rappresenta invece le funzionalità astratte della vista, ad esempio "un elenco di elementi ToDo".

La vista è associata a dati al modello di visualizzazione. Gli aggiornamenti al modello di visualizzazione vengono riflessi automaticamente nella visualizzazione. Le associazioni funzionano anche nell'altra direzione. Gli eventi nel DOM (ad esempio, i clic) sono associati a funzioni nel modello di visualizzazione, che attivano le chiamate AJAX.

Il modello SPA organizza il codice JavaScript lato client in tre livelli:

- todo. DataContext. js: invia richieste AJAX.
- todo. Model. js: definisce i modelli.
- todo. ViewModel. js: definisce il modello di visualizzazione.

![](knockoutjs-template/_static/image11.png)

Questi file di script si trovano nella cartella Scripts/app della soluzione.

![](knockoutjs-template/_static/image12.png)

**todo. DataContext** gestisce tutte le chiamate AJAX ai controller API Web. (Le chiamate AJAX per l'accesso sono definite altrove, in ajaxLogin. js).

**todo. Model. js** definisce i modelli lato client (browser) per gli elenchi di attività. Sono disponibili due classi di modello: todoItem e todo.

Molte delle proprietà nelle classi del modello sono di tipo "Ko. osservabile". Gli osservabili sono il modo in cui viene magicato. Dalla [documentazione di Knockout](http://knockoutjs.com/documentation/introduction.html): un osservabile è un "oggetto JavaScript che può notificare ai Sottoscrittori le modifiche". Quando viene modificato il valore di un oggetto osservabile, Knockout Aggiorna tutti gli elementi HTML associati a tali osservabili. Ad esempio, todoItem presenta osservabili per le proprietà title e Undone:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

È anche possibile sottoscrivere un osservabile nel codice. La classe todoItem, ad esempio, sottoscrive le modifiche apportate alle proprietà "e" e "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Visualizza modello**

Il modello di visualizzazione è definito in todo. ViewModel. js. Il modello di visualizzazione è il punto centrale in cui l'applicazione associa gli elementi della pagina HTML ai dati del dominio. Nel modello SPA il modello di visualizzazione contiene una matrice osservabile di todoLists. Il codice seguente nel modello di visualizzazione indica a knockout di applicare le associazioni:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML e data binding

Il codice HTML principale per la pagina è definito in views/Home/index. cshtml. Poiché si usa il data binding, il codice HTML è solo un modello per ciò di cui viene effettivamente eseguito il rendering. Knockout USA associazioni *dichiarative* . Per associare gli elementi della pagina ai dati, è necessario aggiungere un attributo "data-bind" all'elemento. Di seguito è riportato un esempio molto semplice, tratto dalla documentazione di Knockout:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

In questo esempio, Knockout aggiorna il contenuto dell'elemento **&lt;span&gt;** con il valore di `myItems.count()`. Ogni volta che questo valore viene modificato, Knockout aggiorna il documento.

Knockout fornisce una serie di tipi di binding diversi. Di seguito sono riportate alcune delle associazioni utilizzate nel modello di SPA:

- **foreach**: consente di scorrere un ciclo e applicare lo stesso markup a ogni elemento dell'elenco. Viene usato per eseguire il rendering degli elenchi di attività e degli elementi attività. All'interno di **foreach**, i binding vengono applicati agli elementi dell'elenco.
- **Visible**: usato per impostare la visibilità. Nascondere il markup quando una raccolta è vuota o rendere visibile il messaggio di errore.
- **value**: usato per popolare i valori del form.
- **clic**: associa un evento click a una funzione nel modello di visualizzazione.

## <a name="anti-csrf-protection"></a>Protezione anti-CSRF

La richiesta tra siti falsa (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è attualmente connesso. Per evitare gli attacchi CSRF, ASP.NET MVC usa i *token antifalsificazione*, detti anche token di verifica della richiesta. L'idea è che il server inserisce un token generato in modo casuale in una pagina Web. Quando il client invia dati al server, deve includere questo valore nel messaggio di richiesta.

I token antifalsificazione funzionano perché la pagina dannosa non è in grado di leggere i token dell'utente, a causa dei criteri di stessa origine. I criteri della stessa origine impediscono ai documenti ospitati in due siti diversi di accedere al contenuto.

ASP.NET MVC offre supporto incorporato per i token antifalsificazione, tramite la classe [antifalsificazione](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) e l'attributo [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) . Attualmente questa funzionalità non è incorporata nell'API Web. Tuttavia, il modello SPA include un'implementazione personalizzata per l'API Web. Questo codice viene definito nella classe `ValidateHttpAntiForgeryTokenAttribute`, che si trova nella cartella filtri della soluzione. Per altre informazioni sull'anti-CSRF nell'API Web, vedere [prevenzione degli attacchi di richiesta intersito falsificazione (CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Conclusione

Il modello SPA è progettato per iniziare a scrivere rapidamente applicazioni Web moderne e interattive. Usa la libreria knockout. js per separare la presentazione (markup HTML) dalla logica dei dati e dell'applicazione. Ma Knockout non è l'unica libreria JavaScript che è possibile usare per creare una SPA. Per esplorare altre opzioni, vedere i [modelli di Spa creati dalla community](../templates/index.md).
