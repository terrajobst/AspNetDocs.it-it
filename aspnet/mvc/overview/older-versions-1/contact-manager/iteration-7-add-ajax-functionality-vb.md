---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: '#7 iterazione-aggiungere la funzionalità AJAX (VB) | Microsoft Docs'
author: microsoft
description: Nella settima iterazione, si migliora la velocità di risposta e le prestazioni dell'applicazione mediante l'aggiunta del supporto per AJAX.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: cee2b6e7c7517a1e03ae26d5233fc438857a030c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601654"
---
# <a name="iteration-7--add-ajax-functionality-vb"></a>#7 iterazione-aggiungere la funzionalità AJAX (VB)

[Microsoft](https://github.com/microsoft)

[Scarica codice](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> Nella settima iterazione, si migliora la velocità di risposta e le prestazioni dell'applicazione mediante l'aggiunta del supporto per AJAX.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Compilazione di un'applicazione MVC di gestione dei contatti ASP.NET (VB)

In questa serie di esercitazioni viene creata un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di archiviare i nomi delle informazioni di contatto, i numeri di telefono e gli indirizzi di posta elettronica per un elenco di persone.

L'applicazione viene compilata su più iterazioni. Ogni iterazione migliora gradualmente l'applicazione. L'obiettivo di questo approccio di iterazione multipla è quello di consentire di comprendere il motivo di ogni modifica.

- #1 iterazione: creare l'applicazione. Nella prima iterazione, il gestore contatti viene creato nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base sul database: creazione, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2: rendere l'applicazione un aspetto gradevole. In questa iterazione, si migliora l'aspetto dell'applicazione modificando la pagina master e il foglio di stile CSS predefiniti della visualizzazione MVC ASP.NET.

- #3 iterazione: aggiungere la convalida del modulo. Nella terza iterazione viene aggiunta la convalida dei form di base. Si impedisce agli utenti di inviare un modulo senza completare i campi dei moduli richiesti. Vengono convalidati anche gli indirizzi di posta elettronica e i numeri di telefono.

- Iterazione #4: rendere l'applicazione a regime di controllo libero. In questa quarta iterazione sono disponibili diversi modelli di progettazione software che semplificano la gestione e la modifica dell'applicazione Contact Manager. Ad esempio, si effettua il refactoring dell'applicazione per usare il modello di repository e il modello di inserimento delle dipendenze.

- Iterazione #5-creare unit test. Nella quinta iterazione, l'applicazione viene semplificata per la manutenzione e la modifica mediante l'aggiunta di unit test. Si simulano le classi del modello di dati e si compilano unit test per i controller e la logica di convalida.

- #6 iterazione: usare lo sviluppo basato su test. In questa sesta iterazione vengono aggiunte nuove funzionalità all'applicazione scrivendo unit test prima e scrivendo il codice in base agli unit test. In questa iterazione vengono aggiunti gruppi di contatti.

- #7 iterazione: aggiungere la funzionalità AJAX. Nella settima iterazione, si migliora la velocità di risposta e le prestazioni dell'applicazione mediante l'aggiunta del supporto per AJAX.

## <a name="this-iteration"></a>Questa iterazione

In questa iterazione dell'applicazione Contact Manager è stato refactoring l'applicazione per l'uso di AJAX. Sfruttando Ajax, rendiamo la risposta dell'applicazione. È possibile evitare di eseguire il rendering di un'intera pagina quando è necessario aggiornare solo una determinata area in una pagina.

Verrà eseguito il refactoring della visualizzazione index, in modo che non sia necessario visualizzare nuovamente l'intera pagina ogni volta che un utente seleziona un nuovo gruppo di contatti. Al contrario, quando un utente fa clic su un gruppo di contatti, aggiorniamo semplicemente l'elenco dei contatti e lasciamo il resto della pagina da solo.

Si modificherà anche il modo in cui funziona il collegamento Delete. Invece di visualizzare una pagina di conferma separata, verrà visualizzata una finestra di dialogo di conferma JavaScript. Se si conferma che si desidera eliminare un contatto, viene eseguita un'operazione di eliminazione HTTP sul server per eliminare il record di contatto dal database.

Inoltre, si approfitterà di jQuery per aggiungere effetti di animazione alla visualizzazione dell'indice. Verrà visualizzata un'animazione quando il nuovo elenco di contatti viene recuperato dal server.

Infine, si approfitterà del supporto del framework ASP.NET AJAX per la gestione della cronologia del browser. Verranno creati punti della cronologia ogni volta che viene eseguita una chiamata AJAX per aggiornare l'elenco dei contatti. In questo modo, i pulsanti indietro e avanti del browser funzioneranno.

## <a name="why-use-ajax"></a>Perché usare AJAX?

L'uso di AJAX offre molti vantaggi. Per prima cosa, l'aggiunta di funzionalità AJAX a un'applicazione comporta una migliore esperienza utente. In un'applicazione Web normale, è necessario eseguire il postback della pagina intera al server ogni volta che un utente esegue un'azione. Quando si esegue un'azione, il browser si blocca e l'utente deve attendere il recupero e la visualizzazione dell'intera pagina.

Si tratta di un'esperienza inaccettabile nel caso di un'applicazione desktop. Tradizionalmente, però, abbiamo vissuto questa esperienza utente non valida nel caso di un'applicazione Web, perché non sapevamo che avremmo potuto migliorare. Abbiamo pensato che fosse una limitazione delle applicazioni Web quando, in realtà, era solo una limitazione delle nostre imagini.

In un'applicazione AJAX non è necessario riportare l'esperienza utente in modo da interrompere solo l'aggiornamento di una pagina. In alternativa, è possibile eseguire una richiesta asincrona in background per aggiornare la pagina. Non è possibile forzare l'attesa dell'utente mentre parte della pagina viene aggiornata.

Sfruttando Ajax, è anche possibile migliorare le prestazioni dell'applicazione. Prendere in considerazione la modalità di funzionamento dell'applicazione Contact Manager senza la funzionalità AJAX. Quando si fa clic su un gruppo di contatti, è necessario visualizzare nuovamente l'intera visualizzazione dell'indice. L'elenco dei contatti e l'elenco dei gruppi di contatti devono essere recuperati dal server di database. Tutti questi dati devono essere passati attraverso la rete dal server Web al Web browser.

Una volta aggiunta la funzionalità AJAX all'applicazione, è possibile evitare di rivisualizzare l'intera pagina quando un utente fa clic su un gruppo di contatti. Non è più necessario acquisire i gruppi di contatti dal database. Non è inoltre necessario eseguire il push dell'intera visualizzazione dell'indice attraverso la rete. Sfruttando Ajax, si riduce la quantità di lavoro che il server di database deve eseguire e si riduce la quantità di traffico di rete richiesta dall'applicazione.

## <a name="don-t-be-afraid-of-ajax"></a>Non temere Ajax

Alcuni sviluppatori evitano di usare AJAX perché si preoccupano dei browser di livello inferiore. Desiderano assicurarsi che le applicazioni Web continueranno a funzionare anche se accessibili da un browser che non supporta JavaScript. Poiché AJAX dipende da JavaScript, alcuni sviluppatori evitano di utilizzare AJAX.

Tuttavia, se si è attenti alla modalità di implementazione di AJAX, è possibile creare applicazioni che funzionano con browser sia di livello superiore che di livello inferiore. L'applicazione Contact Manager funzionerà con i browser che supportano JavaScript e browser che non lo sono.

Se si usa l'applicazione Contact Manager con un browser che supporta JavaScript, si avrà un'esperienza utente migliore. Ad esempio, quando si fa clic su un gruppo di contatti, verrà aggiornata solo l'area della pagina che Visualizza i contatti.

Se, invece, si usa l'applicazione Contact Manager con un browser che non supporta JavaScript (o che ha JavaScript disabilitato), si avrà un'esperienza utente leggermente meno appetibile. Quando si fa clic su un gruppo di contatti, ad esempio, è necessario eseguire il postback dell'intera visualizzazione dell'indice al browser per visualizzare l'elenco di contatti corrispondente.

## <a name="adding-the-required-javascript-files"></a>Aggiunta dei file JavaScript richiesti

È necessario usare tre file JavaScript per aggiungere la funzionalità AJAX all'applicazione. Tutti e tre questi file sono inclusi nella cartella Scripts di una nuova applicazione MVC ASP.NET.

Se si prevede di usare AJAX in più pagine dell'applicazione, è opportuno includere i file JavaScript richiesti nella pagina master della visualizzazione dell'applicazione. In questo modo, i file JavaScript verranno inclusi automaticamente in tutte le pagine dell'applicazione.

Aggiungere i seguenti elementi JavaScript inclusi all'interno del &lt;Head&gt; tag della pagina di visualizzazione Master:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactoring della visualizzazione index per l'utilizzo di AJAX

Per iniziare, modificare la visualizzazione dell'indice in modo che facendo clic su un gruppo di contatti venga aggiornata solo l'area della vista che Visualizza i contatti. La casella rossa nella figura 1 contiene l'area che si vuole aggiornare.

[![aggiornare solo i contatti](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Figura 01**: aggiornamento solo di contatti ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-7-add-ajax-functionality-vb/_static/image2.png))

Il primo passaggio consiste nel separare la parte della vista che si desidera aggiornare in modo asincrono in un separato parziale (visualizzazione del controllo utente). La sezione della vista index che visualizza la tabella dei contatti è stata spostata nella parte parziale del listato 1.

**Listato 1-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Si noti che l'oggetto parziale nel listato 1 ha un modello diverso rispetto alla vista index. L'attributo *Inherits* nella direttiva &lt;% @ Page%&gt; specifica che eredita parzialmente dal gruppo di&lt;ViewUserControl&gt; classe.

La vista index aggiornata è inclusa nel listato 2.

**Listato 2-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Ci sono due aspetti da notare sulla visualizzazione aggiornata nel listato 2. Si noti prima di tutto che tutto il contenuto spostato nella parte parziale viene sostituito con una chiamata a HTML. RenderPartial (). Il metodo HTML. RenderPartial () viene chiamato quando la visualizzazione dell'indice viene richiesta per la prima volta in modo da visualizzare il set iniziale di contatti.

In secondo luogo, si noti che il file HTML. ActionLink () usato per visualizzare i gruppi di contatti è stato sostituito con AJAX. ActionLink (). Ajax. ActionLink () viene chiamato con i parametri seguenti:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

Il primo parametro rappresenta il testo da visualizzare per il collegamento, il secondo parametro rappresenta i valori della route e il terzo parametro rappresenta le opzioni Ajax. In questo caso, si usa l'opzione UpdateTargetId Ajax per puntare al tag HTML &lt;div&gt; che si vuole aggiornare al termine della richiesta AJAX. Si vuole aggiornare il tag div&gt; &lt;con il nuovo elenco di contatti.

Il metodo index () aggiornato del controller Contact è contenuto nel listato 3.

**Listato 3-Controllers\ContactController.vb (metodo index)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

L'azione index () aggiornata restituisce in modo condizionale uno dei due elementi. Se l'azione index () viene richiamata da una richiesta AJAX, il controller restituisce un oggetto parziale. In caso contrario, l'azione index () restituisce un'intera visualizzazione.

Si noti che l'azione index () non deve restituire il maggior quantità di dati quando viene richiamata da una richiesta AJAX. Nel contesto di una richiesta normale, l'azione index restituisce un elenco di tutti i gruppi di contatti e del gruppo di contatti selezionato. Nel contesto di una richiesta AJAX, l'azione index () restituisce solo il gruppo selezionato. Ajax significa meno lavoro sul server di database.

La visualizzazione degli indici modificati funziona in caso di browser di livello superiore e di livello inferiore. Se si fa clic su un gruppo di contatti e il browser supporta JavaScript, viene aggiornata solo l'area della visualizzazione che contiene l'elenco dei contatti. Se invece il browser non supporta JavaScript, viene aggiornata l'intera visualizzazione.

La visualizzazione degli indici aggiornata presenta un problema. Quando si fa clic su un gruppo di contatti, il gruppo selezionato non è evidenziato. Poiché l'elenco dei gruppi viene visualizzato al di fuori dell'area aggiornata durante una richiesta AJAX, il gruppo di destra non viene evidenziato. Questo problema verrà risolto nella sezione successiva.

## <a name="adding-jquery-animation-effects"></a>Aggiunta degli effetti di animazione jQuery

In genere, quando si fa clic su un collegamento in una pagina Web, è possibile utilizzare la barra di stato del browser per rilevare se il browser sta recuperando attivamente il contenuto aggiornato. Quando si esegue una richiesta AJAX, d'altra parte, l'indicatore di stato del browser non Mostra lo stato di avanzamento. Questa operazione può rendere gli utenti nervosi. Come è possibile sapere se il browser è stato bloccato?

Esistono diversi modi in cui è possibile indicare a un utente che il lavoro viene eseguito durante l'esecuzione di una richiesta AJAX. Un approccio consiste nel visualizzare un'animazione semplice. Ad esempio, è possibile sfumare un'area quando una richiesta AJAX inizia e si dissolve nell'area quando la richiesta viene completata.

Per creare gli effetti di animazione verrà usata la libreria jQuery inclusa con il Framework di MVC Microsoft ASP.NET. La vista index aggiornata è inclusa nel listato 4.

**Listato 4-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Si noti che la vista index aggiornata contiene tre nuove funzioni JavaScript. Quando si fa clic su un nuovo gruppo di contatti, le prime due funzioni usano jQuery per attenuare e dissolvere l'elenco dei contatti. La terza funzione Visualizza un messaggio di errore quando una richiesta AJAX genera un errore (ad esempio, il timeout di rete).

La prima funzione si occupa anche dell'evidenziazione del gruppo selezionato. Un attributo Class = Selected viene aggiunto all'elemento padre, ovvero l'elemento LI, dell'elemento su cui è stato fatto clic. Anche in questo caso jQuery semplifica la selezione dell'elemento corretto e l'aggiunta della classe CSS.

Questi script sono collegati ai collegamenti di gruppo con la guida del parametro AjaxOptions AJAX. ActionLink (). La chiamata al metodo Ajax. ActionLink () aggiornata ha un aspetto simile al seguente:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Aggiunta del supporto per la cronologia del browser

In genere, quando si fa clic su un collegamento per aggiornare una pagina, viene aggiornata la cronologia del browser. In questo modo, è possibile fare clic sul pulsante indietro del browser per tornare indietro nel tempo allo stato precedente della pagina. Se, ad esempio, si fa clic sul gruppo contatto amici e si fa clic sul gruppo contatti aziendali, è possibile fare clic sul pulsante indietro del browser per tornare allo stato della pagina quando è stato selezionato il gruppo contatto amici.

Sfortunatamente, l'esecuzione di una richiesta AJAX non aggiorna automaticamente la cronologia del browser. Se si fa clic su un gruppo di contatti e l'elenco dei contatti corrispondenti viene recuperato con una richiesta AJAX, la cronologia del browser non viene aggiornata. Non è possibile usare il pulsante indietro del browser per tornare a un gruppo di contatti dopo aver selezionato un nuovo gruppo di contatti.

Se si vuole che gli utenti siano in grado di usare il pulsante indietro del browser dopo aver eseguito le richieste AJAX, è necessario eseguire alcune operazioni. È necessario sfruttare la funzionalità di gestione della cronologia del browser compilata nel framework ASP.NET AJAX.

ASP.NET AJAX browser History, è necessario eseguire tre operazioni:

1. Abilitare la cronologia del browser impostando la proprietà enableBrowserHistory su true.
2. Salva i punti della cronologia quando lo stato di una vista cambia chiamando il Metodo addHistoryPoint ().
3. Ricostruire lo stato della visualizzazione quando viene generato l'evento Navigate.

La vista index aggiornata è inclusa nel listato 5.

**Listato 5-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

Nel listato 5 la cronologia del browser è abilitata nella funzione pageInit (). La funzione pageInit () viene usata anche per configurare il gestore eventi per l'evento Navigate. L'evento Navigate viene generato ogni volta che il pulsante avanti o indietro del browser causa la modifica dello stato della pagina.

Il metodo beginContactList () viene chiamato quando si fa clic su un gruppo di contatti. Questo metodo crea un nuovo punto di cronologia chiamando il Metodo addHistoryPoint (). L'ID del gruppo di contatti selezionato è stato aggiunto alla cronologia.

L'ID del gruppo viene recuperato da un attributo expando nel collegamento del gruppo di contatti. Viene eseguito il rendering del collegamento con la seguente chiamata a Ajax. ActionLink ().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

L'ultimo parametro passato a Ajax. ActionLink () aggiunge un attributo expando denominato GroupID al collegamento (in lettere minuscole per la compatibilità XHTML).

Quando un utente preme il pulsante indietro o avanti del browser, viene generato l'evento Navigate e viene chiamato il metodo Navigate (). Questo metodo aggiorna i contatti visualizzati nella pagina in modo che corrispondano allo stato della pagina che corrisponde al punto della cronologia del browser passato al metodo Navigate.

## <a name="performing-ajax-deletes"></a>Esecuzione di eliminazioni Ajax

Attualmente, per eliminare un contatto, è necessario fare clic sul collegamento Elimina, quindi fare clic sul pulsante Elimina visualizzato nella pagina Conferma eliminazione (vedere la figura 2). Si tratta di una grande quantità di richieste di pagine per eseguire un'operazione semplice, ad esempio l'eliminazione di un record di database.

[![pagina di conferma dell'eliminazione](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Figura 02**: pagina di conferma dell'eliminazione ([fare clic per visualizzare l'immagine con dimensioni complete](iteration-7-add-ajax-functionality-vb/_static/image4.png))

Si sta tentando di ignorare la pagina di conferma dell'eliminazione ed eliminare un contatto direttamente dalla visualizzazione dell'indice. È consigliabile evitare questa tentazione perché l'adozione di questo approccio consente di aprire l'applicazione in caso di problemi di sicurezza. In generale, non si vuole eseguire un'operazione HTTP GET quando si richiama un'azione che modifica lo stato dell'applicazione Web. Quando si esegue un'operazione di eliminazione, si desidera eseguire un'operazione HTTP POST, o meglio ancora, un'operazione HTTP DELETE.

Il collegamento Delete è contenuto nell'oggetto di riferimento parziale. Una versione aggiornata dell'elenco di contatti parziale è contenuta nel listato 6.

**Elenco 6-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

Viene eseguito il rendering del collegamento Delete con la chiamata seguente al metodo Ajax. ImageActionLink ():

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax. ImageActionLink () non è una parte standard di ASP.NET MVC Framework. Ajax. ImageActionLink () è un metodo helper personalizzato incluso nel progetto Contact Manager.

Il parametro AjaxOptions ha due proprietà. Per prima cosa, viene usata la proprietà Confirm per visualizzare una finestra di dialogo di conferma JavaScript Popup. In secondo luogo, per eseguire un'operazione di eliminazione HTTP viene usata la proprietà HttpMethod.

L'elenco 7 contiene una nuova azione AjaxDelete () aggiunta al controller di contatto.

**Listato 7-Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

L'azione AjaxDelete () è decorata con un attributo AcceptVerbs. Questo attributo impedisce che l'azione venga richiamata ad eccezione di qualsiasi operazione HTTP diversa da un'operazione HTTP DELETE. In particolare, non è possibile richiamare questa azione con un HTTP GET.

Dopo aver eliminato il record del database, è necessario visualizzare l'elenco aggiornato dei contatti che non contengono il record eliminato. Il metodo AjaxDelete () restituisce l'elenco dei contatti in modo parziale e aggiornato.

## <a name="summary"></a>Riepilogo

In questa iterazione è stata aggiunta la funzionalità AJAX all'applicazione Contact Manager. È stato usato Ajax per migliorare la velocità di risposta e le prestazioni dell'applicazione.

In primo luogo, è stato eseguito il refactoring della visualizzazione dell'indice in modo che facendo clic su un gruppo di contatti non venga aggiornata l'intera visualizzazione. Se invece si fa clic su un gruppo di contatti, viene aggiornato solo l'elenco dei contatti.

A questo punto, sono stati usati gli effetti di animazione jQuery per attenuare e dissolversi nell'elenco dei contatti. L'aggiunta di un'animazione a un'applicazione AJAX può essere utilizzata per fornire agli utenti dell'applicazione l'equivalente di un indicatore di stato del browser.

È stato inoltre aggiunto il supporto per la cronologia del browser all'applicazione AJAX. È stato consentito agli utenti di fare clic sui pulsanti indietro e indietro del browser per modificare lo stato della visualizzazione dell'indice.

Infine, è stato creato un collegamento Delete che supporta le operazioni di eliminazione HTTP. Eseguendo le eliminazioni Ajax, si consente agli utenti di eliminare i record del database senza richiedere all'utente di richiedere una pagina di conferma dell'eliminazione aggiuntiva.

> [!div class="step-by-step"]
> [Precedente](iteration-6-use-test-driven-development-vb.md)
