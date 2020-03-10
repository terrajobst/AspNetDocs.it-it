---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usare AJAX per distribuire aggiornamenti dinamici | Microsoft Docs
author: microsoft
description: Il passaggio 10 implementa il supporto per gli utenti che hanno eseguito l'accesso per rispondere all'interesse per partecipare a una cena, usando un approccio basato su AJAX integrato nei dettagli della cena...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600849"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a>Usare AJAX per distribuire aggiornamenti dinamici

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 10 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.
> 
> Il passaggio 10 implementa il supporto per gli utenti che hanno eseguito l'accesso per rispondere all'interesse per partecipare a una cena, usando un approccio basato su AJAX integrato nella pagina dei dettagli della cena.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner passaggio 10: abilitazione di RSVP accepts

A questo punto, è possibile implementare il supporto per gli utenti che hanno eseguito l'accesso per rispondere all'interesse per partecipare a una cena. Questa operazione verrà abilitata usando un approccio basato su AJAX integrato nella pagina dei dettagli della cena.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Che indica se l'utente è RSVP

Gli utenti possono visitare l'URL */dinners/details/[ID*] per visualizzare i dettagli relativi a una cena specifica:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Il metodo di azione Details () viene implementato in modo analogo al seguente:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Il primo passaggio per implementare il supporto per RSVP consiste nell'aggiungere un metodo di supporto "IsUserRegistered (username)" all'oggetto Dinner (all'interno della classe parziale Dinner.cs creata in precedenza). Questo metodo helper restituisce true o false a seconda che l'utente sia attualmente RSVP per la cena:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

È quindi possibile aggiungere il codice seguente al modello di vista Details. aspx per visualizzare un messaggio appropriato che indica se l'utente è registrato o meno per l'evento:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Ora, quando un utente visita la cena, viene registrato per visualizzare il messaggio seguente:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Quando visitano una cena, non sono registrati per visualizzare il messaggio seguente:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementazione del metodo di azione Register

A questo punto, aggiungere la funzionalità necessaria per consentire agli utenti di rispondere a una cena dalla pagina dei dettagli.

Per implementare questa operazione, verrà creata una nuova classe "RSVPController" facendo clic con il pulsante destro del mouse sulla directory \Controllers e scegliendo il comando di menu Aggiungi&gt;controller.

Verrà implementato un metodo di azione "Register" all'interno della nuova classe RSVPController che accetta un ID per Dinner come argomento, recupera l'oggetto Dinner appropriato, verifica se l'utente connesso è attualmente nell'elenco degli utenti che hanno eseguito la registrazione e se non aggiunge un oggetto RSVP:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Si noti sopra come viene restituita una stringa semplice come output del metodo di azione. È possibile che questo messaggio sia stato incorporato in un modello di vista, ma poiché è talmente piccolo, si userà semplicemente il metodo helper content () nella classe di base controller e viene restituito un messaggio stringa come sopra.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Chiamata del metodo di azione RSVPForEvent tramite AJAX

Si userà AJAX per richiamare il metodo di azione Register dalla visualizzazione dettagli. L'implementazione di questa operazione è piuttosto semplice. Verranno innanzitutto aggiunti due riferimenti alla libreria di script:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La prima libreria fa riferimento alla libreria di script del lato client ASP.NET AJAX di base. Questo file ha una dimensione di circa 24K (compresso) e contiene la funzionalità AJAX di base sul lato client. La seconda libreria contiene funzioni di utilità che si integrano con i metodi di supporto AJAX incorporati di ASP.NET MVC, che verranno usati a breve.

È quindi possibile aggiornare il codice del modello di visualizzazione aggiunto in precedenza in modo che, invece di inviare un messaggio "non è registrato per questo evento", viene invece eseguito il rendering di un collegamento che, quando viene eseguito il push, esegue una chiamata AJAX che richiama il metodo di azione RSVPForEvent sul controller RSVP e RSVP l'utente:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Il metodo helper Ajax. ActionLink () usato in precedenza è incorporato in ASP.NET MVC ed è simile al metodo helper HTML. ActionLink () con la differenza che anziché eseguire una navigazione standard esegue una chiamata AJAX al metodo di azione quando si fa clic sul collegamento. Sopra viene chiamato il metodo di azione "Register" sul controller "RSVP" e viene passato il DinnerID come parametro "ID". Il parametro AjaxOptions finale che si sta passando indica che si desidera ottenere il contenuto restituito dal metodo di azione e aggiornare l'elemento HTML &lt;div&gt; nella pagina il cui ID è "rsvpmsg".

A questo punto, quando un utente passa a una cena che non è ancora registrato, visualizzerà un collegamento a RSVP:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Se si fa clic sul collegamento "RSVP per questo evento", effettuerà una chiamata AJAX al metodo di azione Register sul controller RSVP e al termine verrà visualizzato un messaggio aggiornato come il seguente:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

La larghezza di banda e il traffico di rete interessati durante l'esecuzione di questa chiamata AJAX sono molto leggeri. Quando l'utente fa clic sul collegamento "RSVP per questo evento", viene apportata una piccola richiesta HTTP POST Network all'URL */dinners/Register/1* , simile a quanto riportato di seguito in transito:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

E la risposta dal metodo di azione Register è semplicemente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Questa chiamata semplice è veloce e funzionerà anche su una rete lenta.

### <a name="adding-a-jquery-animation"></a>Aggiunta di un'animazione jQuery

La funzionalità AJAX implementata è molto efficace e rapida. In alcuni casi, tuttavia, un utente potrebbe non notare che il collegamento RSVP è stato sostituito con il nuovo testo. Per rendere più ovvio il risultato, è possibile aggiungere un'animazione semplice per attirare l'attenzione sul messaggio di aggiornamento.

Il modello di progetto MVC ASP.NET predefinito include jQuery, una libreria JavaScript open source eccellente e molto diffusa, supportata anche da Microsoft. jQuery offre una serie di funzionalità, tra cui una libreria di selezione di DOM HTML ed effetti.

Per usare jQuery, aggiungeremo prima di tutto un riferimento a script. Poiché si userà jQuery in una vasta gamma di posizioni all'interno del sito, si aggiungerà il riferimento allo script nel file della pagina master del sito. Master in modo che tutte le pagine possano usarlo.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Suggerimento: assicurarsi di aver installato l'hotfix JavaScript IntelliSense per VS 2008 SP1 che consente il supporto IntelliSense più completo per i file JavaScript (incluso jQuery). È possibile scaricarlo da: http://tinyurl.com/vs2008javascripthotfix*

Il codice scritto utilizzando JQuery spesso utilizza un metodo JavaScript globale "$ ()" che recupera uno o più elementi HTML utilizzando un selettore CSS. Ad esempio, *$ ("#rsvpmsg")* seleziona qualsiasi elemento HTML con ID rsvpmsg, mentre *$ (". something")* seleziona tutti gli elementi con il nome della classe CSS "Something". È anche possibile scrivere query più avanzate, ad esempio "Restituisci tutti i pulsanti di opzione controllati" usando una query di selezione come: *$ ("input [@type= Radio] [@checked]")* .

Dopo aver selezionato gli elementi, è possibile chiamare i metodi su di essi per eseguire un'azione, ad esempio per nasconderli: *$ ("#rsvpmsg"). Hide ();*

Per lo scenario RSVP, definiamo una semplice funzione JavaScript denominata "AnimateRSVPMessage" che seleziona "rsvpmsg" &lt;div&gt; e aggiunge un'animazione alla dimensione del contenuto di testo. Il codice riportato di seguito avvia il testo di dimensioni ridotte e quindi ne causa l'aumento in un intervallo di tempo di 400 millisecondi:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

È quindi possibile collegare questa funzione JavaScript affinché venga chiamata dopo che la chiamata AJAX è stata completata correttamente passando il nome al metodo helper Ajax. ActionLink () (tramite la proprietà di evento AjaxOptions "onSuccess"):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

A questo punto, quando si fa clic sul collegamento "RSVP per questo evento" e la chiamata AJAX viene completata correttamente, il messaggio di contenuto restituito verrà animato e aumenterà di grandi dimensioni:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Oltre a fornire un evento "onSuccess", l'oggetto AjaxOptions espone gli eventi OnBegin, OnFailure e OnComplete che è possibile gestire, insieme a un'ampia gamma di altre proprietà e opzioni utili.

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Pulizia: effettuare il refactoring di una visualizzazione parziale RSVP

Il modello di visualizzazione dei dettagli sta iniziando a essere un po' più difficile da comprendere. Per contribuire a migliorare la leggibilità del codice, è necessario creare una visualizzazione parziale, RSVPStatus. ascx, che incapsula tutto il codice di visualizzazione RSVP per la pagina dei dettagli.

Questa operazione può essere eseguita facendo clic con il pulsante destro del mouse sulla cartella \Views\Dinners e scegliendo il comando di menu Aggiungi&gt;visualizzazione. Si avrà un oggetto Dinner come ViewModel fortemente tipizzato. È quindi possibile copiare e incollare il contenuto RSVP dalla vista Details. aspx al suo interno.

Una volta completata questa operazione, viene creata anche un'altra visualizzazione parziale, EditAndDeleteLinks. ascx, che incapsula il codice di visualizzazione collegamento modifica ed Elimina. In questo modo, si avrà a disposizione un oggetto Dinner come ViewModel fortemente tipizzato, che verrà copiato e incollato dalla visualizzazione dettagli. aspx.

Il modello di visualizzazione dettagli può includere solo due chiamate al metodo HTML. RenderPartial () nella parte inferiore:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

In questo modo il codice pulisce la lettura e la gestione.

### <a name="next-step"></a>Passo successivo

Esaminiamo ora come è possibile usare ulteriormente AJAX e aggiungere il supporto per il mapping interattivo all'applicazione.

> [!div class="step-by-step"]
> [Precedente](secure-applications-using-authentication-and-authorization.md)
> [Successivo](use-ajax-to-implement-mapping-scenarios.md)
