---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Proteggere le applicazioni usando l'autenticazione e l'autorizzazione | Microsoft Docs
author: microsoft
description: Il passaggio 9 illustra come aggiungere l'autenticazione e l'autorizzazione per proteggere l'applicazione NerdDinner, in modo che gli utenti debbano registrarsi e accedere al sito per crearlo...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8d509c5f15bb4d5014e53b8dc2a736454238e72c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600982"
---
# <a name="secure-applications-using-authentication-and-authorization"></a>Proteggere le applicazioni tramite l'autenticazione e l'autorizzazione

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 9 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.
> 
> Il passaggio 9 illustra come aggiungere l'autenticazione e l'autorizzazione per proteggere l'applicazione NerdDinner, in modo che gli utenti debbano registrarsi e accedere al sito per creare nuove cene e solo l'utente che ospita una cena può modificarlo in un secondo momento.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner passaggio 9: autenticazione e autorizzazione

Al momento l'applicazione NerdDinner concede a chiunque visiti il sito la possibilità di creare e modificare i dettagli di ogni cena. Modificare questa operazione in modo che gli utenti debbano registrarsi e accedere al sito per creare nuove cene e aggiungere una restrizione in modo che solo l'utente che ospita una cena possa modificarlo in un secondo momento.

Per abilitare questa operazione, si useranno l'autenticazione e l'autorizzazione per proteggere l'applicazione.

### <a name="understanding-authentication-and-authorization"></a>Informazioni sull'autenticazione e sull'autorizzazione

L' *autenticazione* è il processo di identificazione e convalida dell'identità di un client che accede a un'applicazione. In altre parole, si tratta di identificare "chi" l'utente finale è quando visita un sito Web. ASP.NET supporta diversi modi per autenticare gli utenti del browser. Per le applicazioni Web Internet, l'approccio di autenticazione più comune usato è denominato "autenticazione basata su form". L'autenticazione basata su form consente a uno sviluppatore di creare un modulo di accesso HTML all'interno dell'applicazione e quindi di convalidare il nome utente e la password inviati da un utente finale a un database o un altro archivio credenziali password. Se la combinazione di nome utente/password è corretta, lo sviluppatore può quindi richiedere a ASP.NET di emettere un cookie HTTP crittografato per identificare l'utente nelle richieste future. Si userà l'autenticazione basata su form con l'applicazione NerdDinner.

L' *autorizzazione* è il processo che consente di determinare se un utente autenticato dispone dell'autorizzazione per accedere a un URL o a una risorsa specifica o per eseguire un'azione. Ad esempio, all'interno dell'applicazione NerdDinner è possibile autorizzare l'accesso all'URL */dinners/create* solo agli utenti che hanno eseguito l'accesso e creare nuove cene. Si vuole anche aggiungere la logica di autorizzazione, in modo che solo l'utente che ospita una cena possa modificarla e negare l'accesso in modifica a tutti gli altri utenti.

### <a name="forms-authentication-and-the-accountcontroller"></a>Autenticazione basata su form e AccountController

Il modello di progetto di Visual Studio predefinito per ASP.NET MVC Abilita automaticamente l'autenticazione basata su form quando vengono create nuove applicazioni MVC ASP.NET. Viene inoltre aggiunta automaticamente un'implementazione della pagina di accesso dell'account predefinita al progetto, che rende molto semplice l'integrazione della sicurezza all'interno di un sito.

La pagina del master default site. master Visualizza il collegamento "Accedi" nella parte superiore destra del sito quando l'utente che esegue l'accesso non è autenticato:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Se si fa clic sul collegamento "Accedi", viene utilizzato un utente per l'URL */account/logon* :

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

I visitatori che non hanno eseguito la registrazione possono farlo facendo clic sul collegamento "Register" (registra), che li porterà all'URL */account/Register* e consentirà di immettere i dettagli dell'account:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Facendo clic sul pulsante "Register", viene creato un nuovo utente all'interno del sistema di appartenenze di ASP.NET e l'utente viene autenticato nel sito utilizzando l'autenticazione basata su form.

Quando un utente è connesso, il sito. master modifica la parte superiore destra della pagina per restituire un "Welcome [username]!" messaggio ed esegue il rendering di un collegamento "Disconnetti" invece di "accesso". Facendo clic sul collegamento "Disconnetti" si disconnette l'utente:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

La funzionalità di accesso, disconnessione e registrazione precedente è implementata all'interno della classe AccountController che è stata aggiunta al progetto da Visual Studio durante la creazione del progetto. L'interfaccia utente per AccountController viene implementata usando i modelli di visualizzazione nella directory \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La classe AccountController usa il sistema di autenticazione basata su form ASP.NET per emettere cookie di autenticazione crittografati e l'API di appartenenza a ASP.NET per archiviare e convalidare nomi utente e password. L'API di appartenenza ASP.NET è estendibile e consente di usare qualsiasi archivio delle credenziali della password. ASP.NET viene fornito con implementazioni del provider di appartenenze predefinite che archiviano nome utente/password all'interno di un database SQL o all'interno Active Directory.

È possibile configurare il provider di appartenenze che l'applicazione NerdDinner deve usare aprendo il file "Web. config" nella radice del progetto e cercando la &lt;appartenenza&gt; sezione al suo interno. Il file Web. config predefinito aggiunto al momento della creazione del progetto registra il provider di appartenenze SQL e lo configura per l'utilizzo di una stringa di connessione denominata "ApplicationServices" per specificare il percorso del database.

La stringa di connessione "ApplicationServices" predefinita, specificata nella sezione &lt;connectionStrings&gt; del file Web. config, è configurata per l'utilizzo di SQL Express. Punta a un database di SQL Express denominato "ASPNETDB. MDF "nella directory" app\_data "dell'applicazione. Se il database non esiste la prima volta che l'API di appartenenza viene utilizzata all'interno dell'applicazione, ASP.NET creerà automaticamente il database ed eseguirà il provisioning dello schema del database di appartenenza appropriato al suo interno:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Se invece di usare SQL Express si vuole usare un'istanza di SQL Server completa (o connettersi a un database remoto), è sufficiente aggiornare la stringa di connessione "ApplicationServices" all'interno del file Web. config e verificare che lo schema di appartenenza appropriato è stato aggiunto al database a cui punta. È possibile eseguire l'utilità "ASPNET\_RegSql. exe" nella directory \Windows\Microsoft.NET\Framework\v2.0.50727\ per aggiungere lo schema appropriato per l'appartenenza e gli altri servizi dell'applicazione ASP.NET a un database.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorizzazione dell'URL/Dinners/Create tramite il filtro [autorizza]

Non è stato necessario scrivere codice per abilitare un'implementazione sicura dell'autenticazione e della gestione degli account per l'applicazione NerdDinner. Gli utenti possono registrare nuovi account con l'applicazione e accedere/disconnettersi dal sito.

È ora possibile aggiungere la logica di autorizzazione all'applicazione e usare lo stato di autenticazione e il nome utente dei visitatori per controllare cosa possono e non possono eseguire nel sito. Per iniziare, aggiungere la logica di autorizzazione ai metodi di azione "Crea" della classe DinnersController. In particolare, sarà necessario che gli utenti che accedono all'URL di */dinners/create* debbano essere connessi. Se non sono connessi, verranno reindirizzati alla pagina di accesso in modo che possano effettuare l'accesso.

L'implementazione di questa logica è piuttosto semplice. È sufficiente aggiungere un attributo di filtro [autorizzate] ai metodi di creazione dell'azione, come indicato di seguito:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC supporta la possibilità di creare "filtri azione" che possono essere usati per implementare la logica riutilizzabile che può essere applicata in modo dichiarativo ai metodi di azione. Il filtro [autorizzazione] è uno dei filtri azione incorporati forniti da ASP.NET MVC e consente a uno sviluppatore di applicare in modo dichiarativo le regole di autorizzazione ai metodi di azione e alle classi controller.

Quando viene applicato senza parametri (come sopra), il filtro [autorizzazione] impone che l'utente che esegue la richiesta del metodo di azione debba essere connesso. in caso contrario, reindirizza automaticamente il browser all'URL di accesso. Quando si esegue questa operazione, l'URL originariamente richiesto viene passato come argomento QueryString, ad esempio:/Account/LogOn? ReturnUrl =% 2fDinners% 2fCreate). Il AccountController reindirizza quindi l'utente all'URL richiesto in origine una volta eseguito l'accesso.

Il filtro [autorizza] supporta facoltativamente la possibilità di specificare una proprietà "Users" o "Roles" che può essere utilizzata per richiedere che l'utente sia connesso e all'interno di un elenco di utenti consentiti o di un membro di un ruolo di sicurezza consentito. Ad esempio, il codice seguente consente solo due utenti specifici, "ScottGu" e "billg", per accedere all'URL/Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

L'incorporamento di nomi utente specifici all'interno del codice tende tuttavia a essere piuttosto non gestibile. Un approccio migliore consiste nel definire i "ruoli" di livello superiore in base ai quali viene eseguita la verifica del codice e quindi eseguire il mapping degli utenti al ruolo utilizzando un database o un sistema Active Directory, consentendo l'archiviazione esterna del codice dell'elenco di mapping utente effettivo. ASP.NET include un'API di gestione dei ruoli incorporata, oltre a un set predefinito di provider di ruoli (inclusi quelli per SQL e Active Directory) che consentono di eseguire il mapping di utenti/ruoli. È quindi possibile aggiornare il codice per consentire solo agli utenti all'interno di un ruolo "admin" specifico di accedere all'URL di/Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Uso della proprietà User.Identity.Name durante la creazione di cene

È possibile recuperare il nome utente dell'utente attualmente connesso di una richiesta usando la proprietà User.Identity.Name esposta nella classe di base del controller.

In precedenza, quando abbiamo implementato la versione HTTP-POST del metodo di azione Create (), avevamo hardcoded la proprietà "HostedBy" della cena in una stringa statica. È ora possibile aggiornare questo codice per usare invece la proprietà User.Identity.Name e aggiungere automaticamente un RSVP per l'host che crea la cena:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Poiché è stato aggiunto un attributo [autorizzate] al metodo Create (), ASP.NET MVC garantisce che il metodo di azione venga eseguito solo se l'utente che visita l'URL/Dinners/Create è connesso al sito. Di conseguenza, il valore della proprietà User.Identity.Name conterrà sempre un nome utente valido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Uso della proprietà User.Identity.Name durante la modifica di cene

Verrà ora aggiunta una logica di autorizzazione che limita gli utenti in modo che possano modificare solo le proprietà delle cene che ospitano.

Per semplificare questa operazione, verrà innanzitutto aggiunto un metodo helper "IsHostedBy (username)" all'oggetto Dinner (all'interno della classe parziale Dinner.cs creata in precedenza). Questo metodo helper restituisce true o false a seconda che il nome utente specificato corrisponda alla proprietà Dinner HostedBy e incapsula la logica necessaria per eseguire un confronto tra stringhe senza distinzione tra maiuscole e minuscole:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Si aggiungerà quindi un attributo [autorizzate] ai metodi di azione Edit () all'interno della classe DinnersController. In questo modo gli utenti devono eseguire l'accesso per richiedere un URL */dinners/Edit/[ID]* .

È quindi possibile aggiungere codice ai metodi di modifica che usano il metodo helper dinner. IsHostedBy (username) per verificare che l'utente connesso corrisponda all'host cena. Se l'utente non è l'host, verrà visualizzata una vista "InvalidOwner" e la richiesta verrà terminata. Il codice a tale scopo è simile al seguente:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

È quindi possibile fare clic con il pulsante destro del mouse sulla directory \Views\Dinners e scegliere il comando di menu Aggiungi-&gt;Visualizza per creare una nuova vista "InvalidOwner". Verrà popolato con il messaggio di errore seguente:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

A questo punto, quando un utente tenta di modificare una cena che non possiede, riceverà un messaggio di errore:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

È possibile ripetere gli stessi passaggi per i metodi di azione Delete () all'interno del controller in modo da bloccare l'autorizzazione per eliminare le cene e assicurarsi che solo l'host di un dinner possa eliminarlo.

### <a name="showinghiding-edit-and-delete-links"></a>Visualizzazione/Nascondi collegamenti di modifica ed eliminazione

È in corso il collegamento al metodo di azione modifica ed Elimina della classe DinnersController dall'URL dei dettagli:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Attualmente vengono visualizzati i collegamenti all'azione modifica ed Elimina, indipendentemente dal fatto che il visitatore dell'URL dei dettagli sia l'host della cena. Modificare questa operazione in modo che i collegamenti vengano visualizzati solo se l'utente che visita è il proprietario della cena.

Il metodo di azione Details () all'interno di DinnersController recupera un oggetto Dinner e quindi lo passa come oggetto modello al modello di visualizzazione:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

È possibile aggiornare il modello di visualizzazione per mostrare o nascondere in modo condizionale i collegamenti Edit ed Delete usando il metodo helper dinner. IsHostedBy () come riportato di seguito:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Passaggi successivi

Si esaminerà ora come è possibile consentire agli utenti autenticati di RSVP per le cene con AJAX.

> [!div class="step-by-step"]
> [Precedente](implement-efficient-data-paging.md)
> [Successivo](use-ajax-to-deliver-dynamic-updates.md)
