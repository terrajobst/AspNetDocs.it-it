---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Informazioni sui trigger UpdatePanel di ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Quando si utilizza l'editor di markup in Visual Studio, è possibile notare (da IntelliSense) che sono presenti due elementi figlio di un controllo UpdatePanel. Uno di WH...
ms.author: riande
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: b1cc869f373d4f8283b4d92af74707c3f11fef61
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588788"
---
# <a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Informazioni sui trigger UpdatePanel di ASP.NET AJAX

di [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Quando si utilizza l'editor di markup in Visual Studio, è possibile notare (da IntelliSense) che sono presenti due elementi figlio di un controllo UpdatePanel. Uno dei quali è l'elemento Triggers, che specifica i controlli nella pagina (o il controllo utente, se ne viene usato uno) che attiverà un rendering parziale del controllo UpdatePanel in cui si trova l'elemento.

## <a name="introduction"></a>Introduzione

La tecnologia ASP.NET di Microsoft offre un modello di programmazione orientato agli oggetti e basato sugli eventi e lo unisce ai vantaggi del codice compilato. Tuttavia, il modello di elaborazione sul lato server presenta diversi svantaggi inerenti alla tecnologia, molti dei quali possono essere trattati dalle nuove funzionalità incluse nelle estensioni Microsoft ASP.NET 3,5 AJAX. Queste estensioni abilitano molte nuove funzionalità rich client, incluso il rendering parziale delle pagine senza richiedere un aggiornamento completo della pagina, la possibilità di accedere ai servizi Web tramite script client (inclusa l'API di profilatura ASP.NET) e un'API lato client estesa progettato per eseguire il mirroring di molti degli schemi di controllo visualizzati nel set di controlli lato server di ASP.NET.

In questo white paper vengono esaminate le funzionalità dei trigger XML del componente `UpdatePanel` ASP.NET AJAX. I trigger XML forniscono un controllo granulare sui componenti che possono causare il rendering parziale di controlli UpdatePanel specifici.

Questo white paper è basato sulla versione beta 2 del .NET Framework 3,5 e Visual Studio 2008. Le estensioni ASP.NET AJAX, in precedenza un assembly del componente aggiuntivo destinate a ASP.NET 2,0, sono ora integrate nella libreria di classi di base .NET Framework. In questo white paper si presuppone inoltre che si lavorerà con Visual Studio 2008, non con Visual Web Developer Express, e verranno fornite procedure dettagliate in base all'interfaccia utente di Visual Studio (anche se gli elenchi di codice saranno completamente compatibili indipendentemente da ambiente di sviluppo).

## <a name="triggers"></a>*Trigger*

Per impostazione predefinita, i trigger per un determinato UpdatePanel includono automaticamente eventuali controlli figlio che richiamano un postback, inclusi i controlli TextBox, ad esempio, che hanno la proprietà `AutoPostBack` impostata su **true**. Tuttavia, i trigger possono essere inclusi anche in modo dichiarativo utilizzando il markup; Questa operazione viene eseguita all'interno della sezione `<triggers>` della dichiarazione di controllo UpdatePanel. Sebbene sia possibile accedere ai trigger tramite la proprietà `Triggers` Collection, è consigliabile registrare tutti i trigger di rendering parziali in fase di esecuzione (ad esempio, se un controllo non è disponibile in fase di progettazione) utilizzando il metodo `RegisterAsyncPostBackControl(Control)` dell'oggetto ScriptManager per la pagina, all'interno dell'evento `Page_Load`. Tenere presente che le pagine sono senza stato, quindi è necessario registrare di nuovo questi controlli ogni volta che vengono creati.

L'inclusione automatica dei trigger figlio può anche essere disabilitata, in modo che i controlli figlio che creano postback non attivino automaticamente i rendering parziali, impostando la proprietà `ChildrenAsTriggers` su **false**. Questo consente la massima flessibilità nell'assegnazione dei controlli specifici che possono richiamare il rendering di una pagina ed è consigliabile, in modo che uno sviluppatore possa acconsentire esplicitamente a rispondere a un evento, anziché gestire gli eventi che possono verificarsi.

Si noti che quando i controlli UpdatePanel sono annidati, quando UpdateMode è impostato su **condizionale**, se l'UpdatePanel figlio viene attivato, ma l'elemento padre non lo è, verrà aggiornato solo l'UpdatePanel figlio. Tuttavia, se l'UpdatePanel padre viene aggiornato, anche l'UpdatePanel figlio verrà aggiornato.

## <a name="the-lttriggersgt-element"></a>*Elemento&gt; &lt;Triggers*

Quando si utilizza l'editor di markup in Visual Studio, è possibile notare (da IntelliSense) che sono presenti due elementi figlio di un controllo `UpdatePanel`. L'elemento visualizzato più di frequente è l'elemento `<ContentTemplate>`, che essenzialmente incapsula il contenuto che verrà mantenuto dal pannello di aggiornamento (il contenuto per il quale viene abilitato il rendering parziale). L'altro elemento è l'elemento `<Triggers>`, che specifica i controlli nella pagina (o il controllo utente, se ne viene usato uno) che attiverà un rendering parziale del controllo UpdatePanel in cui si trova il &lt;trigger&gt; elemento.

L'elemento `<Triggers>` può contenere un numero qualsiasi di due nodi figlio: `<asp:AsyncPostBackTrigger>` e `<asp:PostBackTrigger>`. Entrambi accettano due attributi, `ControlID` e `EventName`e possono specificare qualsiasi controllo all'interno dell'unità di incapsulamento corrente. ad esempio, se il controllo UpdatePanel si trova all'interno di un controllo utente Web, non provare a fare riferimento a un controllo nella pagina in cui risiederà il controllo utente.

L'elemento `<asp:AsyncPostBackTrigger>` è particolarmente utile in quanto può essere destinato a qualsiasi evento da un controllo esistente come figlio di *qualsiasi* controllo UpdatePanel nell'unità di incapsulamento, non solo nell'UpdatePanel in cui il trigger è figlio. Pertanto, qualsiasi controllo può essere eseguito per attivare un aggiornamento parziale della pagina.

Analogamente, è possibile usare l'elemento `<asp:PostBackTrigger>` per attivare un rendering parziale della pagina, ma uno che richiede un round trip completo al server. Questo elemento trigger può essere utilizzato anche per forzare il rendering di una pagina completa quando un controllo altrimenti normalmente attiverà un rendering parziale della pagina, ad esempio quando un controllo `Button` esiste nell'elemento `<ContentTemplate>` di un controllo UpdatePanel. Anche in questo caso, l'elemento PostBackTrigger può specificare qualsiasi controllo figlio di qualsiasi controllo UpdatePanel nell'unità di incapsulamento corrente.

## <a name="lttriggersgt-element-reference"></a>*&lt;trigger&gt; riferimento all'elemento*

*Discendenti markup:*

| **Tag** | **Descrizione** |
| --- | --- |
| &lt;ASP: AsyncPostBackTrigger&gt; | Specifica un controllo e un evento che determineranno un aggiornamento parziale della pagina per UpdatePanel che contiene questo riferimento al trigger. |
| &lt;ASP: PostBackTrigger&gt; | Specifica un controllo e un evento che genereranno un aggiornamento pagina completo (aggiornamento completo della pagina). Questo tag può essere usato per forzare un aggiornamento completo quando un controllo altrimenti attiverà il rendering parziale. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Procedura dettagliata: trigger Cross-UpdatePanel*

1. Crea una nuova pagina ASP.NET con un oggetto ScriptManager impostato per abilitare il rendering parziale. Aggiungere due UpdatePanel a questa pagina: nel primo, includere un controllo Label (Label1) e due controlli Button (Button1 e Button2). Button1 dovrebbe indicare fare clic per aggiornare e Button2 dovrebbe fare clic per aggiornarlo o un elemento lungo tali righe. Nel secondo UpdatePanel, includere solo un controllo Label (Label2), ma impostare la relativa proprietà ForeColor su un valore diverso da quello predefinito per distinguerlo.
2. Impostare la proprietà UpdateMode di entrambi i tag UpdatePanel su **Conditional**.

**Listato 1: markup per default. aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Nel gestore dell'evento click per Button1, impostare Label1. Text e Label2. Text su Something dipendente dal tempo, ad esempio DateTime. Now. ToLongTimeString ()). Per il gestore dell'evento click per Button2, impostare solo Label1. Text sul valore dipendente dall'ora.

**Listato 2: codebehind (Trim) in default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Premere F5 per compilare ed eseguire il progetto. Si noti che, quando si fa clic su Aggiorna entrambi i pannelli, entrambe le etichette cambiano testo; Tuttavia, quando si fa clic su Aggiorna questo pannello, solo gli aggiornamenti Label1.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))

## <a name="under-the-hood"></a>*Dietro le quinte*

Utilizzando l'esempio appena creato, è possibile esaminare il funzionamento di ASP.NET AJAX e la modalità di funzionamento dei trigger tra pannelli UpdatePanel. A tale scopo, si collaborerà con il codice HTML di origine della pagina generato e con l'estensione Mozilla Firefox denominata FireBug-with it, è possibile esaminare facilmente i postback AJAX. Si userà anche lo strumento .NET Reflector di Lutz Roeder. Entrambi questi strumenti sono disponibili gratuitamente online ed è possibile trovarli con una ricerca Internet.

Un esame del codice sorgente della pagina non mostra nulla di comune; i controlli UpdatePanel vengono sottoposti a rendering come `<div>` contenitori ed è possibile visualizzare le risorse di script incluse fornite dal `<asp:ScriptManager>`. Sono inoltre disponibili alcune nuove chiamate specifiche di AJAX a PageRequestManager interne alla libreria di script client AJAX. Infine, vengono visualizzati i due contenitori UpdatePanel: uno con i pulsanti `<input>` sottoposti a rendering con i due controlli `<asp:Label>` sottoposti a rendering come `<span>` contenitori. Se si esamina l'albero DOM in FireBug, si noterà che le etichette sono visualizzate in grigio per indicare che non producono contenuto visibile.

Fare clic sul pulsante Aggiorna questo pannello. si noti che l'UpdatePanel superiore verrà aggiornato con l'ora corrente del server. In FireBug scegliere la scheda console per poter esaminare la richiesta. Esaminare prima i parametri della richiesta POST:

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))

Si noti che UpdatePanel ha indicato al codice AJAX lato server esattamente quale albero dei controlli è stato generato tramite il parametro ScriptManager1: `Button1` del controllo `UpdatePanel1`. A questo punto, fare clic sul pulsante aggiorna entrambi i pannelli. Quindi, esaminando la risposta, viene visualizzata una serie delimitata da pipe di variabili impostate in una stringa. in particolare, viene visualizzato il primo UpdatePanel, `UpdatePanel1`, che contiene l'intero codice HTML inviato al browser. La libreria di script client AJAX sostituisce il contenuto HTML originale di UpdatePanel con il nuovo contenuto tramite la proprietà `.innerHTML`, quindi il server invia il contenuto modificato dal server come HTML.

A questo punto, fare clic sul pulsante aggiorna entrambi i pannelli ed esaminare i risultati del server. I risultati sono molto simili, entrambi gli UpdatePanel ricevono un nuovo codice HTML dal server. Come per il callback precedente, viene inviato lo stato della pagina aggiuntivo.

Come si può notare, poiché non viene utilizzato alcun codice speciale per eseguire un postback AJAX, la libreria di script client AJAX è in grado di intercettare i postback del modulo senza codice aggiuntivo. I controlli server usano automaticamente JavaScript in modo da non inviare automaticamente il modulo. ASP.NET inserisce automaticamente il codice per la convalida e lo stato dei form, principalmente ottenibili con l'inclusione automatica delle risorse script, la classe PostBackOptions , e la classe ClientScriptManager.

Si consideri, ad esempio, un controllo CheckBox; esaminare il disassembly della classe in .NET Reflector. A tale scopo, assicurarsi che l'assembly System. Web sia aperto e passare alla classe `System.Web.UI.WebControls.CheckBox`, aprendo il metodo `RenderInputTag`. Cercare un condizionale che controlla la proprietà `AutoPostBack`:

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))

Quando il postback automatico è abilitato in un controllo `CheckBox` (tramite la proprietà AutoPostBack true), viene pertanto eseguito il rendering del tag `<input>` risultante con uno script di gestione degli eventi di ASP.NET nell'attributo `onclick`. L'intercettazione dell'invio del modulo consente quindi di inserire ASP.NET AJAX nella pagina in modo non intrusivo, evitando potenziali modifiche di rilievo che potrebbero verificarsi mediante una sostituzione di stringa probabilmente imprecisa. Questo consente inoltre a *qualsiasi* controllo ASP.NET personalizzato di sfruttare la potenza di ASP.NET AJAX senza codice aggiuntivo per supportarne l'uso all'interno di un contenitore UpdatePanel.

La funzionalità `<triggers>` corrisponde ai valori inizializzati nella chiamata PageRequestManager per \_updateControls. si noti che la libreria di script client ASP.NET AJAX utilizza la convenzione che i metodi, gli eventi e i nomi di campo che iniziano con un carattere di sottolineatura sono contrassegnati come interni e non sono destinati all'utilizzo all'esterno della libreria stessa. Con esso, possiamo osservare quali controlli sono destinati a causare postback AJAX.

Ad esempio, è possibile aggiungere due controlli aggiuntivi alla pagina, lasciando un controllo al di fuori degli UpdatePanel e lasciandone uno all'interno di un UpdatePanel. Si aggiungerà un controllo CheckBox all'interno dell'UpdatePanel superiore e si rilascia un oggetto DropDownList con un numero di colori definito all'interno dell'elenco. Ecco il nuovo markup:

**Listato 3: nuovo markup**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Ecco il nuovo code-behind:

**Listato 4: codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

L'idea alla base di questa pagina è che l'elenco a discesa seleziona uno dei tre colori per visualizzare la seconda etichetta, che la casella di controllo determina se è in grassetto e se le etichette visualizzano anche la data e l'ora. La casella di controllo non deve causare un aggiornamento AJAX, ma l'elenco a discesa dovrebbe essere, anche se non è ospitato all'interno di un UpdatePanel.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))

Come è evidente nella schermata precedente, il pulsante più recente su cui si fa clic era il pulsante destro aggiorna questo pannello, che ha aggiornato la parte superiore del tempo indipendentemente dal tempo più basso. Anche la data è stata disattivata tra i clic, perché la data è visibile nell'etichetta in basso. Infine, è il colore dell'etichetta bottom: è stato aggiornato più di recente rispetto al testo dell'etichetta, che dimostra che lo stato del controllo è importante e gli utenti si aspettano che venga mantenuto tramite postback AJAX. *Tuttavia*, il tempo non è stato aggiornato. Il tempo è stato ripopolato automaticamente tramite la persistenza del \_\_campo VIEWSTATE della pagina che viene interpretato dal runtime ASP.NET quando è stato eseguito nuovamente il rendering del controllo nel server. Il codice del server ASP.NET AJAX non riconosce in quali metodi cambiano lo stato dei controlli; viene semplicemente ripopolato dallo stato di visualizzazione e quindi esegue gli eventi appropriati.

È tuttavia necessario notare che è stato inizializzato l'ora all'interno della pagina\_evento Load, il tempo sarebbe stato incrementato correttamente. Di conseguenza, gli sviluppatori devono tenere presente che il codice appropriato viene eseguito durante i gestori eventi appropriati ed evitare l'utilizzo del caricamento della pagina\_quando un gestore eventi di controllo sarebbe appropriato.

## <a name="summary"></a>Riepilogo

Il controllo UpdatePanel di ASP.NET AJAX Extensions è versatile e può usare diversi metodi per identificare gli eventi di controllo che ne determineranno l'aggiornamento. Supporta l'aggiornamento automatico da parte dei relativi controlli figlio, ma può anche rispondere agli eventi di controllo in altri punti della pagina.

Per ridurre il potenziale del carico di elaborazione del server, è consigliabile impostare la proprietà `ChildrenAsTriggers` di un UpdatePanel su `false`e che gli eventi siano espliciti anziché inclusi per impostazione predefinita. In questo modo si impedisce anche che gli eventi non necessari causino effetti potenzialmente indesiderati, inclusa la convalida, e le modifiche ai campi di input. Questi tipi di bug potrebbero essere difficili da isolare, perché la pagina viene aggiornata in modo trasparente all'utente e la causa potrebbe quindi non essere immediatamente ovvia.

Esaminando i lavori interni del modello di post-intercettazione di ASP.NET AJAX form, è stato possibile determinare che utilizza il Framework già fornito da ASP.NET. In questo modo, viene mantenuta la massima compatibilità con i controlli progettati utilizzando lo stesso Framework e si inseriscono minime su qualsiasi JavaScript aggiuntivo scritto per la pagina.

## <a name="bio"></a>Biografia

Rob Paveza è uno sviluppatore di applicazioni .NET Senior presso Terralever ([www.Terralever.com](http://www.terralever.com)), una società leader Interactive Marketing a Tempe, AZ. Può essere raggiunto in [robpaveza@gmail.com](mailto:robpaveza@gmail.com)e il suo blog si trova nel [http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/).

Scott Cate collabora con le tecnologie Web Microsoft a partire dal 1997 ed è il Presidente di myKB.com ([www.myKB.com](http://www.myKB.com)), dove si specializza nella scrittura di applicazioni basate su ASP.NET incentrate sulle soluzioni software della Knowledge base. Scott può essere contattato tramite posta elettronica all' [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o al suo blog all' [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Successivo](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
