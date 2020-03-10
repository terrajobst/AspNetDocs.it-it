---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Utilizzare AJAX per implementare scenari di mapping | Microsoft Docs
author: microsoft
description: Il passaggio 11 Mostra come integrare il supporto per il mapping AJAX nell'applicazione NerdDinner, consentendo agli utenti che creano, modificano o visualizzano cene per visualizzare il...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 7fc90f978b9f9eca511feca70a3c0d02ec69b940
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580115"
---
# <a name="use-ajax-to-implement-mapping-scenarios"></a>Usare AJAX per implementare scenari di mapping

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 11 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni, ma completa, usando ASP.NET MVC 1.
> 
> Il passaggio 11 Mostra come integrare il supporto per il mapping AJAX nell'applicazione NerdDinner, consentendo agli utenti che creano, modificano o visualizzano cene per vedere graficamente la posizione della cena.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner passaggio 11: integrazione di una mappa AJAX

L'applicazione verrà ora risulterà più interessante grazie all'integrazione del supporto per i mapping AJAX. Ciò consentirà agli utenti di creare, modificare o visualizzare cene per visualizzare graficamente la posizione della cena.

### <a name="creating-a-map-partial-view"></a>Creazione di una visualizzazione parziale della mappa

Verrà usata la funzionalità di mapping in diverse posizioni all'interno dell'applicazione. Per ridurre il codice, si incapsulano le funzionalità comuni della mappa all'interno di un singolo modello parziale che è possibile riutilizzare tra più azioni e visualizzazioni del controller. La visualizzazione parziale "map. ascx" verrà denominata e creata nella directory \Views\Dinners

È possibile creare il mapping. ascx parziale facendo clic con il pulsante destro del mouse sulla directory \Views\Dinners e scegliendo il comando di menu Aggiungi&gt;visualizzazione. Chiameremo la vista "map. ascx", la assegni come visualizzazione parziale e indicheremo che verrà passata una classe di modello "Dinner" fortemente tipizzata:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Quando si fa clic sul pulsante "Aggiungi", verrà creato il modello parziale. Il file map. ascx verrà quindi aggiornato in modo da includere il contenuto seguente:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

Il primo &lt;script&gt; riferimento punta alla libreria di mapping di Microsoft Virtual Earth 6,2. Il secondo &lt;script&gt; riferimento punta a un file map. js che presto creeremo che incapsulando la logica di mapping JavaScript comune. L'elemento&gt; &lt;div id = "theMap" è il contenitore HTML che verrà usato da Virtual Earth per ospitare la mappa.

È quindi disponibile un blocco&gt; script &lt;incorporato che contiene due funzioni JavaScript specifiche di questa visualizzazione. La prima funzione usa jQuery per collegare una funzione che viene eseguita quando la pagina è pronta per l'esecuzione di script sul lato client. Chiama una funzione helper LoadMap () che verrà definita all'interno del file di script map. js per caricare il controllo mappa Virtual Earth. La seconda funzione è un gestore eventi di callback che aggiunge un pin alla mappa che identifica una posizione.

Si noti come viene usato un blocco di &lt;% =%&gt; sul lato server all'interno del blocco di script lato client per incorporare la latitudine e la longitudine della cena di cui si vuole eseguire il mapping nel codice JavaScript. Si tratta di una tecnica utile per restituire valori dinamici che possono essere usati da script lato client (senza richiedere una chiamata AJAX separata al server per recuperare i valori, rendendola più veloce). I blocchi di &lt;% =%&gt; vengono eseguiti quando viene eseguito il rendering della vista nel server. Pertanto, l'output del codice HTML finirà solo con i valori JavaScript incorporati (ad esempio: var Latitudine = 47,64312;).

### <a name="creating-a-mapjs-utility-library"></a>Creazione di una libreria di utilità map. js

A questo punto, creare il file map. js che è possibile usare per incapsulare la funzionalità JavaScript per la mappa (e implementare i metodi LoadMap e LoadPin sopra indicati). A tale scopo, fare clic con il pulsante destro del mouse sulla directory \Scripts all'interno del progetto, quindi scegliere il comando di menu "Aggiungi&gt;nuovo elemento", selezionare l'elemento JScript e denominarlo "map. js".

Di seguito è riportato il codice JavaScript che verrà aggiunto al file map. js che interagisce con Virtual Earth per visualizzare la mappa e aggiungere i relativi punti per le cene:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrazione della mappa con i moduli di creazione e modifica

Il supporto per la mappa verrà ora integrato con i nostri scenari di creazione e modifica esistenti. La buona notizia è che questa operazione è piuttosto semplice e non richiede la modifica del codice del controller. Poiché le visualizzazioni crea e modifica condividono una visualizzazione parziale "DinnerForm" comune per implementare l'interfaccia utente di Dinner form, è possibile aggiungere la mappa in un'unica posizione e fare in modo che gli scenari di creazione e modifica lo usino.

È sufficiente aprire la visualizzazione parziale \Views\Dinners\DinnerForm.ascx e aggiornarla in modo da includere la nuova mappa come parziale. Di seguito è riportato l'aspetto del DinnerForm aggiornato dopo l'aggiunta della mappa (Nota: gli elementi del modulo HTML vengono omessi dal frammento di codice riportato di seguito per brevità):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Il DinnerForm parziale precedente accetta un oggetto di tipo "DinnerFormViewModel" come tipo di modello (perché è necessario un oggetto Dinner, oltre a un oggetto Select per popolare il DropDownList dei paesi). Per la mappa parziale è necessario solo un oggetto di tipo "Dinner" come tipo di modello. Pertanto, quando si esegue il rendering della mappa partial, viene passata solo la sottoproprietà Dinner di DinnerFormViewModel:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La funzione JavaScript aggiunta al linguaggio parziale usa jQuery per alleghiare un evento "blur" alla casella di testo HTML "Address". Si è probabilmente sentito di "concentrare" gli eventi che vengono generati quando un utente fa clic o tabulazioni in una casella di testo. Il contrario è un evento "blur" che viene attivato quando un utente esce da una casella di testo. Il gestore eventi precedente Cancella i valori della casella di testo Latitudine e Longitudine quando si verifica, quindi traccia la nuova posizione degli indirizzi sulla mappa. Un gestore eventi di callback definito all'interno del file map. js aggiornerà le caselle di testo Longitudine e latitudine nel form usando i valori restituiti da Virtual Earth in base all'indirizzo assegnato.

A questo punto, quando si esegue nuovamente l'applicazione e si fa clic sulla scheda "host Dinner", verrà visualizzata una mappa predefinita con gli elementi standard Dinner:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Quando si digita un indirizzo e quindi si esegue il tabulazione, la mappa verrà aggiornata in modo dinamico per visualizzare il percorso e il gestore eventi compilerà le caselle di testo di Latitudine/Longitudine con i valori di posizione:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Se si salva la nuova cena e la si apre di nuovo per la modifica, si noterà che il percorso della mappa viene visualizzato quando viene caricata la pagina:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Ogni volta che il campo dell'indirizzo viene modificato, vengono aggiornate la mappa e le coordinate di Latitudine/Longitudine.

Ora che la mappa Visualizza la posizione della cena, è anche possibile modificare i campi del form di latitudine e Longitudine dalle caselle di testo visibili in modo da essere elementi nascosti (poiché la mappa viene aggiornata automaticamente ogni volta che viene immesso un indirizzo). A tale scopo, si passerà dall'uso dell'helper HTML. TextBox () HTML all'uso del metodo helper HTML. Hidden ():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Ora i nostri moduli sono un po' più semplici da usare ed evitare di visualizzare la latitudine/longitudine non elaborata (archiviando comunque ogni cena nel database):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrazione della mappa con la visualizzazione dettagli

Ora che la mappa è integrata con gli scenari di creazione e modifica, è possibile integrarla anche con lo scenario dei dettagli. È sufficiente chiamare &lt;% HTML. RenderPartial ("map"); %&gt; nella visualizzazione dettagli.

Di seguito è riportato il codice sorgente per la visualizzazione dettagli completa (con l'integrazione della mappa) simile al seguente:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

A questo punto, quando un utente passa a un URL/Dinners/Details/[ID], visualizzerà i dettagli relativi alla cena, la posizione della cena sulla mappa (completa con un pin di push che, quando si passa il mouse sopra, Visualizza il titolo della cena e l'indirizzo) e dispone di un collegamento AJAX per l'invito:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementazione della ricerca di percorsi nel database e nel repository

Per terminare l'implementazione AJAX, aggiungere una mappa al home page dell'applicazione che consenta agli utenti di cercare graficamente cene vicine.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Si inizierà con l'implementazione del supporto all'interno del database e del livello di repository dei dati per eseguire in modo efficiente una ricerca RADIUS basata sulla posizione per le cene. È possibile usare le nuove [funzionalità geospaziali di SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) per implementarlo. in alternativa, è possibile usare un approccio di funzione SQL che Gary Dryden ha discusso in questo articolo: [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) e Rob Coney bloggato sull'uso di con LINQ to SQL qui: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Per implementare questa tecnica, si aprirà "Esplora server" all'interno di Visual Studio, si seleziona il database NerdDinner e quindi si fa clic con il pulsante destro del mouse sul sottonodo "Functions" sotto di essa e si sceglie di creare una nuova funzione a valori scalari:

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Verrà quindi incollata la funzione allontanarci seguente:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Si creerà quindi una nuova funzione con valori di tabella in SQL Server che chiameremo "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Questa funzione di tabella "NearestDinners" usa la funzione helper allontanarci per restituire tutte le cene entro 100 chilometri dalla latitudine e dalla longitudine fornite:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Per chiamare questa funzione, si aprirà prima di tutto la finestra di progettazione LINQ to SQL facendo doppio clic sul file NerdDinner. dbml nella directory \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Verranno quindi trascinate le funzioni NearestDinners e allontanarci nella finestra di progettazione LINQ to SQL, che ne determinerà l'aggiunta come metodi nella classe LINQ to SQL NerdDinnerDataContext:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

È quindi possibile esporre un metodo di query "FindByLocation" nella classe DinnerRepository che usa la funzione NearestDinner per restituire le cene imminenti entro 100 chilometri dalla posizione specificata:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementazione di un metodo di azione di ricerca AJAX basato su JSON

Ora verrà implementato un metodo di azione del controller che sfrutta il nuovo metodo del repository FindByLocation () per restituire un elenco di dati di Dinner che possono essere usati per popolare una mappa. Questo metodo di azione restituirà i dati della cena in un formato JSON (JavaScript Object Notation), in modo che possa essere facilmente manipolato con JavaScript nel client.

Per implementare questa operazione, verrà creata una nuova classe "SearchController" facendo clic con il pulsante destro del mouse sulla directory \Controllers e scegliendo il comando di menu Aggiungi&gt;controller. Verrà quindi implementato un metodo di azione "SearchByLocation" all'interno della nuova classe SearchController come indicato di seguito:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Il metodo di azione SearchByLocation di SearchController chiama internamente il metodo FindByLocation su DinnerRepository per ottenere un elenco di cene vicine. Invece di restituire gli oggetti dinner direttamente al client, tuttavia, restituisce gli oggetti JsonDinner. La classe JsonDinner espone un subset di proprietà Dinner (ad esempio, per motivi di sicurezza non divulga i nomi degli utenti che hanno risposto a una cena). Include anche una proprietà RSVPCount che non esiste nella cena, che viene calcolata in modo dinamico contando il numero di oggetti RSVP associati a una particolare cena.

Viene quindi usato il metodo helper JSON () sulla classe di base controller per restituire la sequenza di cene usando un formato wire basato su JSON. JSON è un formato di testo standard per la rappresentazione di strutture di dati semplici. Di seguito è riportato un esempio dell'aspetto di un elenco in formato JSON di due oggetti JsonDinner quando viene restituito dal metodo di azione:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Chiamata al metodo AJAX basato su JSON tramite jQuery

A questo punto è possibile aggiornare la home page dell'applicazione NerdDinner per usare il metodo di azione SearchByLocation di SearchController. A tale scopo, aprire il modello di visualizzazione/Views/Home/Index.aspx e aggiornarlo in modo che includa una casella di testo, un pulsante di ricerca, la mappa e un elemento &lt;div&gt; denominato dinnert:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

È quindi possibile aggiungere due funzioni JavaScript alla pagina:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La prima funzione JavaScript carica la mappa quando la pagina viene caricata per la prima volta. La seconda funzione JavaScript collega un gestore dell'evento Click JavaScript nel pulsante Search. Quando si preme il pulsante, viene chiamata la funzione JavaScript FindDinnersGivenLocation () che verrà aggiunta al file map. js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Questa funzione FindDinnersGivenLocation () chiama map. Trovare () sul controllo Virtual Earth per centrarlo sul percorso immesso. Quando il servizio Virtual Earth Map restituisce, la mappa. Il metodo Find () richiama il metodo di callback callbackUpdateMapDinners che è stato passato come argomento finale.

Il metodo callbackUpdateMapDinners () è il punto in cui viene eseguito il lavoro effettivo. Usa il metodo helper $. post () di jQuery per eseguire una chiamata AJAX al metodo di azione SearchByLocation () di SearchController, passando la latitudine e la longitudine della mappa appena centrata. Definisce una funzione inline che verrà chiamata quando il metodo helper $. post () viene completato e i risultati della cena in formato JSON restituiti dal metodo di azione SearchByLocation () verranno passati usando una variabile denominata "dinners". Esegue quindi un'istruzione foreach su ogni cena restituita e usa la latitudine e la longitudine della cena e altre proprietà per aggiungere un nuovo PIN alla mappa. Viene inoltre aggiunta una voce di cena all'elenco HTML di cene a destra della mappa. Quindi collega un evento hover per l'elenco puntine da disegno e HTML in modo da visualizzare i dettagli relativi alla cena quando un utente passa il mouse su di essi:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

A questo punto, quando si esegue l'applicazione e si visita la Home page, viene visualizzata una mappa. Quando si immette il nome di una città, la mappa visualizzerà le cene imminenti vicine:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Se si passa il mouse su una cena, vengono visualizzati i relativi dettagli.

Facendo clic sul titolo della cena nella bolla o sul lato destro dell'elenco HTML, si passerà alla cena, che è possibile RSVP per:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Passo successivo

Ora abbiamo implementato tutte le funzionalità dell'applicazione NerdDinner. Si esaminerà ora come è possibile abilitare il testing unità automatizzato.

> [!div class="step-by-step"]
> [Precedente](use-ajax-to-deliver-dynamic-updates.md)
> [Successivo](enable-automated-unit-testing.md)
