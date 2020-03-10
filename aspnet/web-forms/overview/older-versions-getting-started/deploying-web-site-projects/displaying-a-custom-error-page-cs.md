---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Visualizzazione di una pagina di erroreC#personalizzata () | Microsoft Docs
author: rick-anderson
description: Che cosa Visualizza l'utente quando si verifica un errore di runtime in un'applicazione Web ASP.NET? La risposta dipende dal modo in cui il sito Web &lt;la configurazione di customErrors&gt;....
ms.author: riande
ms.date: 06/09/2009
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: c1ff4c112b9a489b8fb9ef3443663cd71eda7965
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78524206"
---
# <a name="displaying-a-custom-error-page-c"></a>Visualizzazione di una pagina di errore personalizzata (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Che cosa Visualizza l'utente quando si verifica un errore di runtime in un'applicazione Web ASP.NET? La risposta dipende dal modo in cui il sito Web &lt;la configurazione di customErrors&gt;. Per impostazione predefinita, agli utenti viene mostrata una schermata gialla che richiede che si sia verificato un errore di Runtime. Questa esercitazione illustra come personalizzare queste impostazioni per visualizzare una pagina di errore esteticamente gradevole che corrisponda all'aspetto del sito.

## <a name="introduction"></a>Introduzione

In un mondo perfetto non ci sarebbero errori di run-time. I programmatori scriveranno il codice con un bug e con una valida convalida dell'input dell'utente e le risorse esterne, come i server di database e i server di posta elettronica, non passano mai offline. Naturalmente, gli errori in realtà sono inevitabili. Le classi nella .NET Framework segnalano un errore generando un'eccezione. Ad esempio, la chiamata al metodo Open di un oggetto SqlConnection stabilisce una connessione al database specificato da una stringa di connessione. Tuttavia, se il database è inattivo o se le credenziali nella stringa di connessione non sono valide, il metodo Open genera un'`SqlException`. Le eccezioni possono essere gestite tramite l'utilizzo di blocchi di `try/catch/finally`. Se il codice all'interno di un blocco di `try` genera un'eccezione, il controllo viene trasferito al blocco catch appropriato, in cui lo sviluppatore può tentare di risolvere l'errore. Se non è presente alcun blocco catch corrispondente o se il codice che ha generato l'eccezione non è in un blocco try, l'eccezione filtra lo stack di chiamate nella ricerca dei blocchi di `try/catch/finally`.

Se l'eccezione si propaga fino al runtime di ASP.NET senza essere gestita, viene generato l' [evento`Error`](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) della [classe`HttpApplication`](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)e viene visualizzata la *pagina di errore* configurata. Per impostazione predefinita, ASP.NET Visualizza una pagina di errore che viene affettuosamente definita [schermata gialla della morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Sono disponibili due versioni di YSOD: una mostra i dettagli dell'eccezione, un'analisi dello stack e altre informazioni utili per gli sviluppatori che eseguono il debug dell'applicazione (vedere la **Figura 1**). l'altro indica semplicemente che si è verificato un errore in fase di esecuzione (vedere la **Figura 2**).

I dettagli dell'eccezione YSOD sono molto utili per gli sviluppatori che esegue il debug dell'applicazione, ma la visualizzazione di un YSOD agli utenti finali è appiccicosa e non professionale. Al contrario, gli utenti finali devono essere portati a una pagina di errore che mantiene l'aspetto del sito con una prosa più intuitiva che descrive la situazione. L'aspetto positivo è che la creazione di una pagina di errore personalizzata è piuttosto semplice. Questa esercitazione inizia con un'occhiata a ASP. Pagine di errore diverse di NET. Viene quindi illustrato come configurare l'applicazione Web per mostrare agli utenti una pagina di errore personalizzata in caso di errore.

### <a name="examining-the-three-types-of-error-pages"></a>Esame dei tre tipi di pagine di errore

Quando si verifica un'eccezione non gestita in un'applicazione ASP.NET, viene visualizzato uno dei tre tipi di pagine di errore seguenti:

- Schermata gialla Dettagli eccezione della pagina errore di morte,
- Schermo di errore di runtime giallo della pagina di errore della morte oppure
- Pagina di errore personalizzata

Gli sviluppatori che hanno familiarità con la pagina di errore sono i dettagli dell'eccezione YSOD. Per impostazione predefinita, questa pagina viene visualizzata agli utenti che visitano localmente e pertanto è la pagina visualizzata quando si verifica un errore durante il test del sito nell'ambiente di sviluppo. Come suggerisce il nome, i dettagli dell'eccezione YSOD forniscono informazioni dettagliate sull'eccezione, ovvero il tipo, il messaggio e l'analisi dello stack. Se l'eccezione è stata generata dal codice nella classe code-behind della pagina ASP.NET e se l'applicazione è configurata per il debug, i dettagli dell'eccezione YSOD mostreranno anche questa riga di codice (e alcune righe di codice sopra e sotto di esso).

**Nella figura 1** è illustrata la pagina Dettagli eccezione YSOD. Prendere nota dell'URL nella finestra dell'indirizzo del browser: `http://localhost:62275/Genre.aspx?ID=foo`. Si ricordi che la pagina `Genre.aspx` elenca le revisioni del libro in un determinato genere. Richiede che `GenreId` valore (un `uniqueidentifier`) venga passato tramite la QueryString; ad esempio, l'URL appropriato per visualizzare le recensioni di fiction è `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Se un valore non`uniqueidentifier` viene passato tramite la QueryString (ad esempio "foo") viene generata un'eccezione.

> [!NOTE]
> Per riprodurre questo errore nell'applicazione Web demo disponibile per il download, è possibile visitare direttamente `Genre.aspx?ID=foo` oppure fare clic sul collegamento "genera un errore di runtime" nella `Default.aspx`.

Si notino le informazioni sull'eccezione presentate nella **Figura 1**. Il messaggio di eccezione "conversione non riuscita durante la conversione da una stringa di caratteri a uniqueidentifier" è presente nella parte superiore della pagina. Viene elencato anche il tipo di eccezione, `System.Data.SqlClient.SqlException`. È disponibile anche la traccia dello stack.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Figura 1**: i dettagli dell'eccezione YSOD includono informazioni sull'eccezione  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-custom-error-page-cs/_static/image3.png))

L'altro tipo di YSOD è l'errore di runtime YSOD ed è illustrato nella **Figura 2**. L'errore di runtime YSOD informa il visitatore che si è verificato un errore di run-time, ma non include alcuna informazione sull'eccezione generata. In questo modo, tuttavia, vengono fornite istruzioni su come rendere visibili i dettagli dell'errore modificando il file `Web.config`, che fa parte di un aspetto YSOD non professionale.

Per impostazione predefinita, viene visualizzato l'errore di runtime YSOD agli utenti che visitano in remoto (tramite http://www.yoursite.com), come evidenziato dall'URL nella barra degli indirizzi del browser nella **Figura 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Esistono due schermate YSOD diverse perché gli sviluppatori sono interessati a conoscere i dettagli dell'errore, ma tali informazioni non devono essere visualizzate in un sito attivo perché potrebbero rivelare potenziali vulnerabilità alla sicurezza o altre informazioni riservate a chiunque visiti il sito.

> [!NOTE]
> Se si segue e si usa DiscountASP.NET come host Web, è possibile notare che l'errore di runtime YSOD non viene visualizzato quando si visita il sito Live. Questo perché DiscountASP.NET ha i server configurati per visualizzare i dettagli dell'eccezione YSOD per impostazione predefinita. La novità è che è possibile eseguire l'override di questo comportamento predefinito aggiungendo una sezione `<customErrors>` al file di `Web.config`. La sezione "configurazione della pagina di errore visualizzata" esamina in dettaglio la sezione `<customErrors>`.

[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Figura 2**: l'errore di runtime YSOD non include dettagli sull'errore  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-custom-error-page-cs/_static/image6.png))

Il terzo tipo di pagina di errore è la pagina di errore personalizzata, ovvero una pagina Web creata dall'utente. Il vantaggio di una pagina di errore personalizzata è che si ha il controllo completo sulle informazioni visualizzate all'utente insieme all'aspetto della pagina; la pagina di errore personalizzata può utilizzare la stessa pagina master e gli stessi stili delle altre pagine. La sezione "utilizzo di una pagina di errore personalizzata" illustra la creazione di una pagina di errore personalizzata e la relativa configurazione per la visualizzazione in caso di eccezione non gestita. La **Figura 3** offre un picco di questa pagina di errore personalizzata. Come si può notare, l'aspetto della pagina di errore è molto più professionale rispetto a una delle schermate gialle della morte illustrata nelle figure 1 e 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Figura 3**: una pagina di errore personalizzata offre un aspetto più personalizzato  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-custom-error-page-cs/_static/image9.png))

Esaminare la barra degli indirizzi del browser nella **Figura 3**. Si noti che la barra degli indirizzi Mostra l'URL della pagina di errore personalizzata (`/ErrorPages/Oops.aspx`). Nelle figure 1 e 2 le schermate gialle della morte vengono visualizzate nella stessa pagina da cui ha avuto origine l'errore (`Genre.aspx`). Alla pagina di errore personalizzata viene passato l'URL della pagina in cui si è verificato l'errore tramite il `aspxerrorpath` parametro QueryString.

## <a name="configuring-which-error-page-is-displayed"></a>Configurazione della pagina di errore visualizzata

Quale delle tre possibili pagine di errore viene visualizzato si basa su due variabili:

- Le informazioni di configurazione nella sezione `<customErrors>` e
- Indica se l'utente visita il sito localmente o in remoto.

La [sezione`<customErrors>`](https://msdn.microsoft.com/library/h0hfz6fc.aspx) in `Web.config` dispone di due attributi che influiscono sulla pagina di errore visualizzata: `defaultRedirect` e `mode`. L'attributo `defaultRedirect` è facoltativo. Se specificato, specifica l'URL della pagina di errore personalizzata e indica che deve essere visualizzata la pagina di errore personalizzata anziché l'errore di runtime YSOD. L'attributo `mode` è obbligatorio e accetta uno dei tre valori seguenti: `On`, `Off`o `RemoteOnly`. Questi valori hanno il seguente comportamento:

- `On`: indica che la pagina di errore personalizzata o l'errore di runtime YSOD viene visualizzato a tutti i visitatori, indipendentemente dal fatto che siano locali o remoti.
- `Off`: specifica che i dettagli dell'eccezione YSOD vengono visualizzati a tutti i visitatori, indipendentemente dal fatto che siano locali o remoti.
- `RemoteOnly`: indica che la pagina di errore personalizzata o l'errore di runtime YSOD viene visualizzato nei visitatori remoti, mentre i dettagli dell'eccezione YSOD vengono visualizzati ai visitatori locali.

A meno che non si specifichi diversamente, ASP.NET funge da se è stato impostato l'attributo mode su `RemoteOnly` e non è stato specificato un valore `defaultRedirect`. In altre parole, il comportamento predefinito è che i dettagli dell'eccezione YSOD vengono visualizzati nei visitatori locali mentre viene visualizzato l'errore di runtime YSOD per i visitatori remoti. È possibile eseguire l'override di questo comportamento predefinito aggiungendo una sezione `<customErrors>` al `Web.config file.` dell'applicazione Web

## <a name="using-a-custom-error-page"></a>Utilizzo di una pagina di errore personalizzata

Ogni applicazione Web deve disporre di una pagina di errore personalizzata. Offre un'alternativa più professionale all'errore di runtime YSOD, è facile da creare e la configurazione dell'applicazione per l'uso della pagina di errore personalizzata richiede solo pochi istanti. Il primo passaggio consiste nella creazione della pagina di errore personalizzata. Ho aggiunto una nuova cartella all'applicazione Book Reviews denominata `ErrorPages` e aggiunta a tale nuova pagina ASP.NET denominata `Oops.aspx`. Fare in modo che la pagina usi la stessa pagina master del resto delle pagine del sito, in modo che erediti automaticamente lo stesso aspetto.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Figura 4**: creare una pagina di errore personalizzata

Successivamente, dedicare alcuni minuti alla creazione del contenuto per la pagina di errore. È stata creata una pagina di errore personalizzata piuttosto semplice con un messaggio che indica che si è verificato un errore imprevisto e un collegamento alla Home page del sito.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Figura 5**: progettare la pagina di errore personalizzata  
 ([Fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-custom-error-page-cs/_static/image14.png))

Con la pagina di errore completata, configurare l'applicazione Web in modo che utilizzi la pagina di errore personalizzata anziché l'errore di runtime YSOD. Questa operazione viene eseguita specificando l'URL della pagina di errore nell'attributo `defaultRedirect` della sezione `<customErrors>`. Aggiungere il markup seguente al file di `Web.config` dell'applicazione:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Il markup precedente configura l'applicazione in modo da visualizzare i dettagli dell'eccezione YSOD agli utenti che visitano localmente, usando la pagina di errore personalizzata Oops. aspx per gli utenti che visitano in remoto. Per verificare questo comportamento, distribuire il sito Web nell'ambiente di produzione e quindi visitare la pagina Genre. aspx nel sito Live con un valore QueryString non valido. Verrà visualizzata la pagina di errore personalizzata (fare riferimento alla **Figura 3**).

Per verificare che la pagina di errore personalizzata venga visualizzata solo agli utenti remoti, visitare la pagina `Genre.aspx` con una QueryString non valida dall'ambiente di sviluppo. È comunque necessario visualizzare i dettagli dell'eccezione YSOD (vedere di nuovo la **Figura 1**). L'impostazione `RemoteOnly` garantisce agli utenti che visitano il sito nell'ambiente di produzione di visualizzare la pagina di errore personalizzata mentre gli sviluppatori che lavorano localmente continuano a vedere i dettagli dell'eccezione.

## <a name="notifying-developers-and-logging-error-details"></a>Notifica degli sviluppatori e registrazione dei dettagli degli errori

Gli errori che si verificano nell'ambiente di sviluppo sono stati causati dallo sviluppatore seduto sul suo computer. Mostra le informazioni sull'eccezione nei dettagli dell'eccezione YSOD e sa quali passaggi ha eseguito quando si è verificato l'errore. Tuttavia, quando si verifica un errore in fase di produzione, lo sviluppatore non è a conoscenza del verificarsi di un errore, a meno che l'utente finale che visita il sito non sia in grado di segnalare l'errore. Anche se l'utente non è in grado di avvisare il team di sviluppo che si è verificato un errore, senza conoscere il tipo di eccezione, il messaggio e la traccia dello stack, potrebbe essere difficile diagnosticare la cause dell'errore, risolverlo.

Per questi motivi, è fondamentale che qualsiasi errore nell'ambiente di produzione venga registrato in un archivio permanente, ad esempio un database, e che gli sviluppatori siano avvisati di questo errore. La pagina di errore personalizzata può sembrare un posto ideale per eseguire questa registrazione e una notifica. Sfortunatamente, la pagina di errore personalizzata non ha accesso ai dettagli dell'errore e pertanto non può essere usata per registrare queste informazioni. L'aspetto positivo è che esistono diversi modi per intercettare i dettagli dell'errore e registrarli e le prossime tre esercitazioni esplorano questo argomento in modo più dettagliato.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Utilizzo di pagine di errore personalizzate diverse per stati di errore HTTP diversi

Quando un'eccezione viene generata da una pagina ASP.NET e non viene gestita, l'eccezione filtra fino al runtime di ASP.NET, che visualizza la pagina di errore configurata. Se una richiesta entra nel motore ASP.NET ma non può essere elaborata per qualche motivo, ad esempio se il file richiesto non è stato trovato o le autorizzazioni di lettura sono state disabilitate per il file, il motore ASP.NET genera un `HttpException`. Questa eccezione, come le eccezioni generate dalle pagine ASP.NET, viene ribolle fino al runtime, causando la visualizzazione della pagina di errore appropriata.

Ciò significa che per l'applicazione Web in produzione è che se un utente richiede una pagina non trovata, verrà visualizzata la pagina di errore personalizzata. **Nella figura 6** viene illustrato un esempio di questo tipo. Poiché la richiesta è relativa a una pagina inesistente (`NoSuchPage.aspx`), viene generata una `HttpException` e viene visualizzata la pagina di errore personalizzata (si noti il riferimento al `NoSuchPage.aspx` nel parametro `aspxerrorpath` QueryString).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Figura 6**: il runtime ASP.NET Visualizza la pagina di errore configurata in risposta a una richiesta non valida ([fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-custom-error-page-cs/_static/image17.png))

Per impostazione predefinita, tutti i tipi di errori provocano la visualizzazione della stessa pagina di errore personalizzata. Tuttavia, è possibile specificare una pagina di errore personalizzata diversa per un codice di stato HTTP specifico usando `<error>` elementi figlio all'interno della sezione `<customErrors>`. Per visualizzare, ad esempio, una pagina di errore diversa in caso di errore di pagina non trovata, che ha un codice di stato HTTP 404, aggiornare la sezione `<customErrors>` in modo da includere il markup seguente:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Con questa modifica, ogni volta che un utente che visita in remoto richiede una risorsa ASP.NET che non esiste, verrà reindirizzato al `404.aspx` pagina di errore personalizzata anziché `Oops.aspx`. Come illustrato **nella figura 7** , nella pagina `404.aspx` possono essere inclusi messaggi più specifici rispetto alla pagina di errore personalizzata generale.

> [!NOTE]
> Consultare le [pagine di errore 404, una volta](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) per informazioni aggiuntive sulla creazione di pagine di errore 404 valide.

[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)

**Figura 7**: la pagina di errore 404 personalizzata Visualizza un messaggio più mirato rispetto a `Oops.aspx`  
([Fare clic per visualizzare l'immagine con dimensioni complete](displaying-a-custom-error-page-cs/_static/image20.png)) 

Poiché è noto che la pagina `404.aspx` viene raggiunta solo quando l'utente effettua una richiesta per una pagina non trovata, è possibile migliorare questa pagina di errore personalizzata per includere la funzionalità per consentire all'utente di risolvere questo tipo di errore specifico. Ad esempio, è possibile compilare una tabella di database che esegue il mapping degli URL non validi noti agli URL corretti e quindi fare in modo che la `404.aspx` pagina di errore personalizzata esegua una query sulla tabella e suggerisca le pagine che l'utente potrebbe tentare di raggiungere.

> [!NOTE]
> La pagina di errore personalizzata viene visualizzata solo quando viene effettuata una richiesta a una risorsa gestita dal motore ASP.NET. Come illustrato nelle [differenze principali tra IIS e l'esercitazione server di sviluppo ASP.NET](core-differences-between-iis-and-the-asp-net-development-server-cs.md) , il server Web può gestire determinate richieste. Per impostazione predefinita, il server Web IIS elabora le richieste di contenuto statico, ad esempio immagini e file HTML senza richiamare il motore ASP.NET. Di conseguenza, se l'utente richiede un file di immagine inesistente, verrà restituito il messaggio di errore 404 predefinito di IIS anziché ASP. Pagina di errore configurata per NET.

## <a name="summary"></a>Riepilogo

Quando si verifica un'eccezione non gestita in un'applicazione ASP.NET, all'utente viene visualizzata una delle tre pagine di errore: la schermata dei dettagli dell'eccezione è gialla. Schermata di morte gialla di errore di runtime; o una pagina di errore personalizzata. Quale pagina di errore viene visualizzata dipende dalla configurazione di `<customErrors>` dell'applicazione e dal fatto che l'utente visiti localmente o in remoto. Il comportamento predefinito consiste nel mostrare i dettagli dell'eccezione YSOD ai visitatori locali e l'errore di runtime YSOD ai visitatori remoti.

Mentre l'errore di runtime YSOD nasconde informazioni potenzialmente riservate sull'errore dell'utente che visita il sito, si interrompe dall'aspetto del sito e fa in modo che l'applicazione sembri bacato. Un approccio migliore consiste nell'usare una pagina di errore personalizzata, che comporta la creazione e la progettazione della pagina di errore personalizzata e l'impostazione dell'URL nell'attributo `defaultRedirect` della sezione `<customErrors>`. È anche possibile avere più pagine di errore personalizzate per diversi Stati di errore HTTP.

La pagina di errore personalizzata è il primo passaggio di una strategia di gestione degli errori completa per un sito Web nell'ambiente di produzione. Anche l'invio di avvisi per lo sviluppatore dell'errore e la registrazione dei relativi dettagli è un passaggio importante. Nelle tre esercitazioni successive vengono esplorate le tecniche per la notifica e la registrazione degli errori.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Pagine di errore, una volta](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Linee guida di progettazione delle eccezioni](https://msdn.microsoft.com/library/ms229014.aspx)
- [Pagine degli errori semplici da usare](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Gestione e generazione di eccezioni](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Uso corretto di pagine di errore personalizzate in ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Precedente](strategies-for-database-development-and-deployment-cs.md)
> [Successivo](processing-unhandled-exceptions-cs.md)
