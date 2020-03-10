---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementare il paging dei dati efficiente | Microsoft Docs
author: microsoft
description: Il passaggio 8 Mostra come aggiungere il supporto per il paging all'URL di/dinners in modo che, invece di visualizzare migliaia di cene contemporaneamente, verranno visualizzate solo 10 cene imminenti all'indirizzo...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 2d9a0dba381b71755ac626f76d52bc5bcb434447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601052"
---
# <a name="implement-efficient-data-paging"></a>Implementare una suddivisione in pagine efficiente dei dati

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 8 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni, ma completa, usando ASP.NET MVC 1.
> 
> Il passaggio 8 Mostra come aggiungere il supporto per il paging all'URL di/dinners in modo che, invece di visualizzare migliaia di cene in una sola volta, verranno visualizzate solo 10 cene imminenti alla volta e gli utenti finali potranno eseguire il paging e l'invio attraverso l'intero elenco in modo intuitivo.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-8-paging-support"></a>NerdDinner passaggio 8: supporto per il paging

Se il sito ha esito positivo, avrà migliaia di cene imminenti. È necessario assicurarsi che l'interfaccia utente venga ridimensionata per gestire tutte le cene e consente agli utenti di esplorarli. Per abilitare questa operazione, verrà aggiunto il supporto per il paging all'URL */dinners* , in modo che invece di visualizzare migliaia di cene in una sola volta, verranno visualizzate solo 10 cene imminenti alla volta e gli utenti finali potranno eseguire il paging e l'invio attraverso l'intero elenco in modo intuitivo.

### <a name="index-action-method-recap"></a>Riepilogo metodo di azione index ()

Il metodo di azione index () all'interno della classe DinnersController ha un aspetto simile al seguente:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Quando viene effettuata una richiesta all'URL */dinners* , viene recuperato un elenco di tutte le cene imminenti e quindi viene eseguito il rendering di un elenco di tutte le cene:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iqueryablelttgt"></a>Informazioni su IQueryable&lt;T&gt;

*IQueryable&lt;t&gt;* è un'interfaccia introdotta con LINQ come parte di .NET 3,5. Consente di implementare scenari di "esecuzione posticipata" avanzati che possono essere usati per implementare il supporto per il paging.

In DinnerRepository viene restituito un oggetto IQueryable&lt;&gt; sequenza dinner dal metodo FindUpcomingDinners ():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

IQueryable&lt;Dinner&gt; oggetto restituito dal metodo FindUpcomingDinners () incapsula una query per recuperare gli oggetti dinner dal database usando LINQ to SQL. In particolare, non esegue la query sul database fino a quando non si tenta di accedere/scorrere i dati nella query o fino a quando non viene chiamato il metodo ToList (). Il codice che chiama il metodo FindUpcomingDinners () può facoltativamente scegliere di aggiungere ulteriori filtri o operazioni "concatenate" all'oggetto IQueryable&lt;Dinner&gt; prima di eseguire la query. LINQ to SQL è quindi sufficientemente intelligente da eseguire la query combinata sul database quando vengono richiesti i dati.

Per implementare la logica di paging è possibile aggiornare il metodo di azione index () di DinnersController in modo da applicare gli operatori "Skip" e "Take" aggiuntivi all'oggetto IQueryable restituito&lt;Dinner&gt; Sequence prima di chiamare ToList () su di esso:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Il codice precedente ignora le prime 10 cene imminenti nel database e quindi restituisce 20 cene. LINQ to SQL è sufficientemente intelligente da creare una query SQL ottimizzata che esegue questa logica ignorata nel database SQL, e non nel server Web. Ciò significa che, anche se ci sono milioni di cene imminenti nel database, solo i 10 desideri verranno recuperati come parte di questa richiesta (rendendola efficiente e scalabile).

### <a name="adding-a-page-value-to-the-url"></a>Aggiunta di un valore "Page" all'URL

Anziché impostare come hardcoded un intervallo di pagine specifico, è opportuno che gli URL includano un parametro "Page" che indichi l'intervallo di cene che un utente sta richiedendo.

#### <a name="using-a-querystring-value"></a>Uso di un valore QueryString

Il codice riportato di seguito illustra come è possibile aggiornare il metodo di azione index () per supportare un parametro QueryString e abilitare URL come */dinners? Page = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Il metodo di azione index () precedente presenta un parametro denominato "Page". Il parametro viene dichiarato come intero Nullable, ovvero quale int? indica. Ciò significa che l'URL di */dinners? Page = 2* provocherà il passaggio di un valore "2" come valore del parametro. L'URL */dinners* (senza un valore querystring) provocherà il passaggio di un valore null.

Il valore di pagina viene moltiplicato per le dimensioni della pagina (in questo caso 10 righe) per determinare il numero di cene da ignorare. Si sta usando l' [ C# operatore di "Unione" null (??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) che risulta utile quando si gestiscono i tipi nullable. Il codice precedente assegna alla pagina il valore 0 se il parametro della pagina è null.

#### <a name="using-embedded-url-values"></a>Uso di valori di URL incorporati

Un'alternativa all'utilizzo di un valore QueryString consiste nell'incorporare il parametro della pagina all'interno dell'URL effettivo. Ad esempio: */dinners/Page/2* o */dinners/2*. ASP.NET MVC include un potente motore di routing degli URL che consente di semplificare il supporto di scenari come questo.

È possibile registrare regole di routing personalizzate che consentono di eseguire il mapping di qualsiasi URL in ingresso o formato URL a qualsiasi classe controller o metodo di azione desiderato. È sufficiente aprire il file Global. asax nel progetto:

![](implement-efficient-data-paging/_static/image2.png)

E quindi registrare una nuova regola di mapping usando il metodo helper MapRoute () come la prima chiamata alle route. MapRoute () di seguito:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Sopra è stata registrata una nuova regola di routing denominata "UpcomingDinners". Viene indicato che il formato dell'URL è "dinners/Page/{Page}", dove {page} è un valore di parametro incorporato nell'URL. Il terzo parametro del metodo MapRoute () indica che è necessario eseguire il mapping degli URL che corrispondono a questo formato al metodo di azione index () nella classe DinnersController.

È possibile utilizzare esattamente lo stesso codice indice () in precedenza con lo scenario QueryString, ad eccezione del fatto che il parametro "Page" proverrà dall'URL e non dalla stringa QueryString:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

A questo punto, quando si esegue l'applicazione e si digita */dinners* , verranno visualizzate le prime 10 cene future:

![](implement-efficient-data-paging/_static/image3.png)

Quando digito */dinners/Page/1* , verrà visualizzata la pagina di cene successiva:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Aggiunta dell'interfaccia utente per la navigazione delle pagine

L'ultimo passaggio per completare lo scenario di paging consiste nell'implementare l'interfaccia utente di navigazione "Avanti" e "precedente" all'interno del modello di visualizzazione per consentire agli utenti di ignorare facilmente i dati di Dinner.

Per implementare correttamente questa operazione, è necessario ottenere informazioni sul numero totale di cene nel database, oltre a quante pagine di dati vengono convertite in. Sarà quindi necessario calcolare se il valore "Page" attualmente richiesto si trova all'inizio o alla fine dei dati e mostrare o nascondere di conseguenza l'interfaccia utente "precedente" e "successiva". È possibile implementare questa logica all'interno del metodo di azione index (). In alternativa, è possibile aggiungere una classe helper al progetto che incapsula questa logica in modo più riutilizzabile.

Di seguito è riportata una semplice classe helper "PaginatedList" che deriva dall'elenco&lt;T&gt; classe Collection incorporata nel .NET Framework. Implementa una classe di raccolta riutilizzabile che può essere utilizzata per impaginare qualsiasi sequenza di dati IQueryable. Nell'applicazione NerdDinner è possibile utilizzare IQueryable&lt;Dinner&gt; results, ma potrebbe essere utilizzato con la stessa facilità con IQueryable&lt;Product&gt; o IQueryable&lt;Customer&gt; risultati in altri scenari di applicazione:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Si noti che sopra il modo in cui vengono calcolate e quindi esposte proprietà quali "PageIndex", "PageSize", "TotalCount" e "TotalPages". Espone inoltre due proprietà Helper "HasPreviousPage" e "HasNextPage" che indicano se la pagina di dati nella raccolta si trova all'inizio o alla fine della sequenza originale. Il codice precedente comporterà l'esecuzione di due query SQL, la prima per recuperare il conteggio del numero totale di oggetti Dinner (che non restituisce gli oggetti, bensì un'istruzione "SELECT COUNT" che restituisce un Integer) e la seconda per recuperare solo le righe di dati necessari per il database per la pagina di dati corrente.

È quindi possibile aggiornare il metodo helper DinnersController. index () per creare un PaginatedList&lt;Dinner&gt; dal risultato DinnerRepository. FindUpcomingDinners () e passarlo al modello di visualizzazione:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

È quindi possibile aggiornare il modello di vista \Views\Dinners\Index.aspx in modo da ereditare da ViewPage&lt;NerdDinner. Helpers. PaginatedList&lt;Dinner&gt;&gt; anziché ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;e quindi aggiungere il codice seguente nella parte inferiore di View-template per mostrare o nascondere l'interfaccia utente di navigazione successiva e precedente:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Si noti sopra come si usa il metodo helper HTML. RouteLink () per generare i collegamenti ipertestuali. Questo metodo è simile al metodo helper HTML. ActionLink () usato in precedenza. La differenza consiste nel fatto che l'URL viene generato usando la regola di routing "UpcomingDinners" che viene impostata nel file Global. asax. In questo modo si garantisce che gli URL vengano generati nel metodo di azione index () con il formato: */dinners/Page/{Page}* , dove il valore {page} è una variabile che viene offerta in precedenza in base all'oggetto PageIndex corrente.

A questo punto, quando si esegue nuovamente l'applicazione, si vedranno 10 cene alla volta nel browser:

![](implement-efficient-data-paging/_static/image5.png)

Sono disponibili anche &lt;&lt;&lt; e &gt;&gt;interfaccia utente di navigazione &gt; nella parte inferiore della pagina che ci permette di saltare i dati in avanti e indietro usando gli URL accessibili del motore di ricerca:

![](implement-efficient-data-paging/_static/image6.png)

| **Argomento laterale: informazioni sulle implicazioni di IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; è una funzionalità molto potente che consente un'ampia gamma di scenari di esecuzione posticipati interessanti, ad esempio query basate su paging e composizione. Come per tutte le funzionalità avanzate, è opportuno prestare attenzione al modo in cui viene usato e assicurarsi che non venga usato. È importante riconoscere che la restituzione di un oggetto IQueryable&lt;T&gt; risultato dal repository consente di chiamare il codice per accodare i metodi degli operatori concatenati, quindi partecipare all'esecuzione della query finale. Se non si desidera fornire al codice chiamante questa possibilità, è necessario restituire IList&lt;T&gt; o IEnumerable&lt;T&gt; results, che contengono i risultati di una query già eseguita. Per gli scenari di impaginazione è necessario eseguire il push della logica di impaginazione dei dati effettiva nel metodo del repository chiamato. In questo scenario è possibile aggiornare il metodo del Finder FindUpcomingDinners () per avere una firma che ha restituito un PaginatedList: PaginatedList&lt; Dinner&gt; FindUpcomingDinners (int pageIndex, int pageSize) {} o restituire un IList&lt;Dinner&gt;e usare un parametro out "totalCount" per restituire il conteggio totale delle cene: IList&lt;Dinner&gt; FindUpcomingDinners (int pageIndex, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Passo successivo

Si esaminerà ora come è possibile aggiungere il supporto per l'autenticazione e l'autorizzazione all'applicazione.

> [!div class="step-by-step"]
> [Precedente](re-use-ui-using-master-pages-and-partials.md)
> [Successivo](secure-applications-using-authentication-and-authorization.md)
